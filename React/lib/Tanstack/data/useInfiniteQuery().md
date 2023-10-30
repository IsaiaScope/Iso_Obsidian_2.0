- _Infinite scroll_
  - fetch new data "just in time" as user scrolls
  - more efficient than fetching all data at once
- Fetch new data when...
  - user clicks a button
  - user scrolls to certain point on the page

---

_useInfiniteQuery_

- Requires different API format than pagination
- Hence new project!
- Pagination
  - track current page in component state
  - new query updates page number
- useInfiniteQuery tracks next query
  - next query is returned as part of the data

```json
{
	"count": 37,
	"next": "http://swapi.dev/api/species/?page=2",
	"previous": null,
	"results": [...]
}
```

---

_Shape of useInfiniteQuery Data_

- Shape of data different than useQuery
- Object with two properties:
  - pages
  - pageParams
- Every query has its own element in the pages array
- pageParams tracks the keys of queries that have been retrieved
  - rarely used, won't use here

_useInfiniteQuery Syntax_

- pageParam is a parameter passed to the queryfn
  - `useInfiniteQuery ("sw-people", ({ pageParam = defaultUrl }) => fetchUrl (pageParam)`
  - Current value of pageParam is maintained by React Query
- useInfiniteQuery options
  - getNextPageParam: (lastPage, allPages)
    - Updates pageParam
    - Might use all of the pages of data (a 11Pages)
    - we will use just the lastPage of data (specifically the next property)

_useInfiniteQuery Return Object Properties_

- fetchNextPage
  - function to call when the user needs more data
- hasNextPage
  - Based on return value of getNextPageParam
  - If undefined, no more data
- isFetchingNextPage
  - For displaying a loading spinner
  - We'll see an example of when it's useful to distinguish between isFetching and isFetchingNextPage
- [[flow-infinitescroll.png | IMG of InfiniteScroll Flow]]

---

_React Infinite Scroller_
- Works really nicely with useInfiniteQuery 
	- [NPM | react-infinite-scroller](https://www.npmjs.com/package/react-infinite-scroller)
- Populate two props for InfiniteScroll component:
	- loadMore= {fetchNextPage}
	- hasMore= (hasNextPage}
- Component takes care of detecting when to load more 
- Data in data. pages [x]. results


---

```jsx
import { useInfiniteQuery } from "@tanstack/react-query";
import InfiniteScroll from "react-infinite-scroller";

import { Person } from "./Person";

const initialUrl = "https://swapi.dev/api/people/";
const fetchUrl = async (url) => {
	const response = await fetch(url);
	return response.json();
};

export function InfinitePeople() {
	const {
		data,
		fetchNextPage,
		hasNextPage,
		isLoading,
		isFetching,
		isError,
		error,
	} = useInfiniteQuery(
		["sw-people"],
		({ pageParam = initialUrl }) => fetchUrl(pageParam),
		{
			getNextPageParam: (lastPage) => lastPage.next || undefined,
		}
	);

	if (isLoading) return <div className="loading">Loading...</div>;
	if (isError) return <div>Error! {error.toString()}</div>;

	return (
		<>
			{isFetching && <div className="loading">Loading...</div>}
			<InfiniteScroll loadMore={fetchNextPage} hasMore={hasNextPage}>
				{data.pages.map((pageData) => {
					return pageData.results.map((person) => {
						return (
							<Person
								key={person.name}
								name={person.name}
								hairColor={person.hair_color}
								eyeColor={person.eye_color}
							/>
						);
					});
				})}
			</InfiniteScroll>
		</>
	);
}
```
