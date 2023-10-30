- [DOC of TanStack](https://tanstack.com/query/latest/docs/react/overview)
- [DOC of TanStack Installation](https://tanstack.com/query/latest/docs/react/installation)
- [DOC of TanStack Quick Start](https://tanstack.com/query/latest/docs/react/quick-start?from=reactQueryV3&original=https%3A%2F%2Ftanstack.com%2Fquery%2Fv3%2Fdocs%2Fquick-start)
- [DOC f TansStack Dev Tool](https://react-query-v3.tanstack.com/devtools#_top)

---

- _Feature_
  - isError => destructed from useQuery() is populated if response throw an error
  - cashing => RQ cash data and behind the scene fetch the same data, meanwhile display the cashed data
    - scaleData => manage when behind scene call happen
    - gcTime => garbage collector time, how long data is stored in the cash
    - staleTime => the cash data is reused if is not old as the time expressed from staleTime (useful to avoid redundant calls before a certain amount of time)
- _useQuery()_ is not retriggered by the changing data, use useState to avoid this behaviour

  - queryFn => function triggered by useQuery => queryFn: ({ signal, queryKey }) => fetchEvents({ signal, ...queryKey[1] }),
    - signal => useful for know status or abort of a call, passed to fetch consent to abort a call when cange page for ex
  - enable => set isPending to true when is disabled, useful when insert spinner under some circostanze
  - isLoanding vs isPending => isLoading is not true when query is disabled

- _useMutation()_ => every call that aren't get
  - mutate() => destructed from useMutation() is the function that pass data to function declared in mutationFn
  - onSuccess => excute when query successed
  - onMutate => called when is callled mutate()
- _queryClient.invalidateQueries({ queryKey: ['events'] })_ => invalidate query cause to refetch data from queryes that includes 'events' key, can be added exact property if only 'event' key must be present
  - reFetch :none => remove automatic refetch, and redo api call when it's needed
  - cancelQueries & getQueryData & setQueryData can use to manipolate cashdata and annul api calls, keeping old data stored instead new one

```jsx
const { mutate } = useMutation({
	mutationFn: updateEvent,
	onMutate: async (data) => {
		const newEvent = data.event;
		await queryClient.cancelQueries({ queryKey: ["events", params.id] });
		const previousEvent = queryClient.getQueryData(["events", params.id]);

		queryClient.setQueryData(["events", params.id], newEvent);
		return { previousEvent };
	},
	onError: (error, data, context) => {
		queryClient.setQueryData(["events", params.id], context.previousEvent);
	},
	onSettled: () => {
		queryClient.invalidateQueries(["events", params.id]);
	},
});
```

---

- perform api call when outside a component, btw is better to use the hook for extra feature that offer as isLoading, ecc..

```jsx
export function loader({ params }) {
	return queryClient.fetchQuery({
		queryKey: ["events", params.id],
		queryFn: ({ signal }) => fetchEvent({ signal, id: params.id }),
	});
}
```

- like point 7 u can mix React router and RQ but in a component is better, on the other hand RR permits to prefetch data with loader and use actions.
- another important and useful hook is useIsFetching => is greater than 0 is RQ is fatching data somewhere in the application, useful for global loader
- [[RQ_1.png | IMG of Cache Query Data]]

---

```jsx
// NewEventsSection
import { useQuery } from "@tanstack/react-query";

import LoadingIndicator from "../UI/LoadingIndicator.jsx";
import ErrorBlock from "../UI/ErrorBlock.jsx";
import EventItem from "./EventItem.jsx";
import { fetchEvents } from "../../util/http.js";

export default function NewEventsSection() {
	const { data, isPending, isError, error } = useQuery({
		queryKey: ["events", { max: 3 }],
		queryFn: ({ signal, queryKey }) => fetchEvents({ signal, ...queryKey[1] }),
		staleTime: 5000,
		// gcTime: 1000
	});

	let content;

	if (isPending) {
		content = <LoadingIndicator />;
	}

	if (isError) {
		content = (
			<ErrorBlock
				title="An error occurred"
				message={error.info?.message || "Failed to fetch events."}
			/>
		);
	}

	if (data) {
		content = (
			<ul className="events-list">
				{data.map((event) => (
					<li key={event.id}>
						<EventItem event={event} />
					</li>
				))}
			</ul>
		);
	}

	return (
		<section className="content-section" id="new-events-section">
			<header>
				<h2>Recently added events</h2>
			</header>
			{content}
		</section>
	);
}
```

---

```jsx
//app
import {
	Navigate,
	RouterProvider,
	createBrowserRouter,
} from "react-router-dom";
import { QueryClientProvider } from "@tanstack/react-query";

import Events from "./components/Events/Events.jsx";
import EventDetails from "./components/Events/EventDetails.jsx";
import NewEvent from "./components/Events/NewEvent.jsx";
import EditEvent, {
	loader as editEventLoader,
	action as editEventAction,
} from "./components/Events/EditEvent.jsx";
import { queryClient } from "./util/http.js";

const router = createBrowserRouter([
	{
		path: "/",
		element: <Navigate to="/events" />,
	},
	{
		path: "/events",
		element: <Events />,
		children: [
			{
				path: "/events/new",
				element: <NewEvent />,
			},
		],
	},
	{
		path: "/events/:id",
		element: <EventDetails />,
		children: [
			{
				path: "/events/:id/edit",
				element: <EditEvent />,
				loader: editEventLoader,
				action: editEventAction,
			},
		],
	},
]);

function App() {
	return (
		<QueryClientProvider client={queryClient}>
			<RouterProvider router={router} />
		</QueryClientProvider>
	);
}

export default App;
```

---

```jsx
//http
import { QueryClient } from "@tanstack/react-query";

export const queryClient = new QueryClient();

export async function fetchEvents({ signal, searchTerm, max }) {
	let url = "http://localhost:3000/events";

	if (searchTerm && max) {
		url += "?search=" + searchTerm + "&max=" + max;
	} else if (searchTerm) {
		url += "?search=" + searchTerm;
	} else if (max) {
		url += "?max=" + max;
	}

	const response = await fetch(url, { signal: signal });

	if (!response.ok) {
		const error = new Error("An error occurred while fetching the events");
		error.code = response.status;
		error.info = await response.json();
		throw error;
	}

	const { events } = await response.json();

	return events;
}

export async function createNewEvent(eventData) {
	const response = await fetch(`http://localhost:3000/events`, {
		method: "POST",
		body: JSON.stringify(eventData),
		headers: {
			"Content-Type": "application/json",
		},
	});

	if (!response.ok) {
		const error = new Error("An error occurred while creating the event");
		error.code = response.status;
		error.info = await response.json();
		throw error;
	}

	const { event } = await response.json();

	return event;
}

export async function fetchSelectableImages({ signal }) {
	const response = await fetch(`http://localhost:3000/events/images`, {
		signal,
	});

	if (!response.ok) {
		const error = new Error("An error occurred while fetching the images");
		error.code = response.status;
		error.info = await response.json();
		throw error;
	}

	const { images } = await response.json();

	return images;
}

export async function fetchEvent({ id, signal }) {
	const response = await fetch(`http://localhost:3000/events/${id}`, {
		signal,
	});

	if (!response.ok) {
		const error = new Error("An error occurred while fetching the event");
		error.code = response.status;
		error.info = await response.json();
		throw error;
	}

	const { event } = await response.json();

	return event;
}

export async function deleteEvent({ id }) {
	const response = await fetch(`http://localhost:3000/events/${id}`, {
		method: "DELETE",
	});

	if (!response.ok) {
		const error = new Error("An error occurred while deleting the event");
		error.code = response.status;
		error.info = await response.json();
		throw error;
	}

	return response.json();
}

export async function updateEvent({ id, event }) {
	const response = await fetch(`http://localhost:3000/events/${id}`, {
		method: "PUT",
		body: JSON.stringify({ event }),
		headers: {
			"Content-Type": "application/json",
		},
	});

	if (!response.ok) {
		const error = new Error("An error occurred while updating the event");
		error.code = response.status;
		error.info = await response.json();
		throw error;
	}

	return response.json();
}
```

---

```jsx
//FindEventSection
import { useRef, useState } from "react";
import { useQuery } from "@tanstack/react-query";

import { fetchEvents } from "../../util/http.js";
import LoadingIndicator from "../UI/LoadingIndicator.jsx";
import ErrorBlock from "../UI/ErrorBlock.jsx";
import EventItem from "./EventItem.jsx";

export default function FindEventSection() {
	const searchElement = useRef();
	const [searchTerm, setSearchTerm] = useState(); // retrigger useQuery

	const { data, isLoading, isError, error } = useQuery({
		queryKey: ["events", { searchTerm: searchTerm }],
		queryFn: ({ signal, queryKey }) => fetchEvents({ signal, ...queryKey[1] }),
		enabled: searchTerm !== undefined,
	});

	function handleSubmit(event) {
		event.preventDefault();
		setSearchTerm(searchElement.current.value);
	}

	let content = <p>Please enter a search term and to find events.</p>;

	if (isLoading) {
		content = <LoadingIndicator />;
	}

	if (isError) {
		content = (
			<ErrorBlock
				title="An error occurred"
				message={error.info?.message || "Failed to fetch events."}
			/>
		);
	}

	if (data) {
		content = (
			<ul className="events-list">
				{data.map((event) => (
					<li key={event.id}>
						<EventItem event={event} />
					</li>
				))}
			</ul>
		);
	}

	return (
		<section className="content-section" id="all-events-section">
			<header>
				<h2>Find your next event!</h2>
				<form onSubmit={handleSubmit} id="search-form">
					<input
						type="search"
						placeholder="Search events"
						ref={searchElement}
					/>
					<button>Search</button>
				</form>
			</header>
			{content}
		</section>
	);
}
```

---

```jsx
//newEvent
import { Link, useNavigate } from "react-router-dom";
import { useMutation } from "@tanstack/react-query";

import Modal from "../UI/Modal.jsx";
import EventForm from "./EventForm.jsx";
import { createNewEvent } from "../../util/http.js";
import ErrorBlock from "../UI/ErrorBlock.jsx";
import { queryClient } from "../../util/http.js";

export default function NewEvent() {
	const navigate = useNavigate();

	const { mutate, isPending, isError, error } = useMutation({
		mutationFn: createNewEvent,
		onSuccess: () => {
			queryClient.invalidateQueries({ queryKey: ["events"] });
			navigate("/events");
		},
	});

	function handleSubmit(formData) {
		mutate({ event: formData });
	}

	return (
		<Modal onClose={() => navigate("../")}>
			<EventForm onSubmit={handleSubmit}>
				{isPending && "Submitting..."}
				{!isPending && (
					<>
						<Link to="../" className="button-text">
							Cancel
						</Link>
						<button type="submit" className="button">
							Create
						</button>
					</>
				)}
			</EventForm>
			{isError && (
				<ErrorBlock
					title="Failed to create event"
					message={
						error.info?.message ||
						"Failed to create event. Please check your inputs and try again later."
					}
				/>
			)}
		</Modal>
	);
}
```

---

```jsx
// newEvent
import { Link, useNavigate } from "react-router-dom";
import { useMutation } from "@tanstack/react-query";

import Modal from "../UI/Modal.jsx";
import EventForm from "./EventForm.jsx";
import { createNewEvent } from "../../util/http.js";
import ErrorBlock from "../UI/ErrorBlock.jsx";
import { queryClient } from "../../util/http.js";

export default function NewEvent() {
	const navigate = useNavigate();

	const { mutate, isPending, isError, error } = useMutation({
		mutationFn: createNewEvent,
		onSuccess: () => {
			queryClient.invalidateQueries({ queryKey: ["events"] });
			navigate("/events");
		},
	});

	function handleSubmit(formData) {
		mutate({ event: formData });
	}

	return (
		<Modal onClose={() => navigate("../")}>
			<EventForm onSubmit={handleSubmit}>
				{isPending && "Submitting..."}
				{!isPending && (
					<>
						<Link to="../" className="button-text">
							Cancel
						</Link>
						<button type="submit" className="button">
							Create
						</button>
					</>
				)}
			</EventForm>
			{isError && (
				<ErrorBlock
					title="Failed to create event"
					message={
						error.info?.message ||
						"Failed to create event. Please check your inputs and try again later."
					}
				/>
			)}
		</Modal>
	);
}
```

---

```jsx
// editevent
import {
	Link,
	redirect,
	useNavigate,
	useParams,
	useSubmit,
	useNavigation,
} from "react-router-dom";
import { useQuery } from "@tanstack/react-query";

import Modal from "../UI/Modal.jsx";
import EventForm from "./EventForm.jsx";
import { fetchEvent, updateEvent, queryClient } from "../../util/http.js";
import ErrorBlock from "../UI/ErrorBlock.jsx";

export default function EditEvent() {
	const navigate = useNavigate();
	const { state } = useNavigation();
	const submit = useSubmit();
	const params = useParams();

	const { data, isError, error } = useQuery({
		queryKey: ["events", params.id],
		queryFn: ({ signal }) => fetchEvent({ signal, id: params.id }),
		staleTime: 10000,
	});

	// const { mutate } = useMutation({
	//   mutationFn: updateEvent,
	//   onMutate: async (data) => {
	//     const newEvent = data.event;

	//     await queryClient.cancelQueries({ queryKey: ['events', params.id] });
	//     const previousEvent = queryClient.getQueryData(['events', params.id]);

	//     queryClient.setQueryData(['events', params.id], newEvent);

	//     return { previousEvent };
	//   },
	//   onError: (error, data, context) => {
	//     queryClient.setQueryData(['events', params.id], context.previousEvent);
	//   },
	//   onSettled: () => {
	//     queryClient.invalidateQueries(['events', params.id]);
	//   },
	// });

	function handleSubmit(formData) {
		submit(formData, { method: "PUT" });
	}

	function handleClose() {
		navigate("../");
	}

	let content;

	if (isError) {
		content = (
			<>
				<ErrorBlock
					title="Failed to load event"
					message={
						error.info?.message ||
						"Failed to load event. Please check your inputs and try again later."
					}
				/>
				<div className="form-actions">
					<Link to="../" className="button">
						Okay
					</Link>
				</div>
			</>
		);
	}

	if (data) {
		content = (
			<EventForm inputData={data} onSubmit={handleSubmit}>
				{state === "submitting" ? (
					<p>Sending data...</p>
				) : (
					<>
						<Link to="../" className="button-text">
							Cancel
						</Link>
						<button type="submit" className="button">
							Update
						</button>
					</>
				)}
			</EventForm>
		);
	}

	return <Modal onClose={handleClose}>{content}</Modal>;
}

export function loader({ params }) {
	return queryClient.fetchQuery({
		queryKey: ["events", params.id],
		queryFn: ({ signal }) => fetchEvent({ signal, id: params.id }),
	});
}

export async function action({ request, params }) {
	const formData = await request.formData();
	const updatedEventData = Object.fromEntries(formData);
	await updateEvent({ id: params.id, event: updatedEventData });
	await queryClient.invalidateQueries(["events"]);
	return redirect("../");
}
```
