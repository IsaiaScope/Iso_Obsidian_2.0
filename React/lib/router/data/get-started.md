1. [lib](https://reactrouter.com/en/main)
2. [GitHubs source](https://github.com/academind/react-complete-guide-code/tree/20-building-mpas-with-react-router-updated/code/32-finished/frontend/src)
3. features
	1. end => useful for "/" path in navLink for set active just when "/" is matched and not always
	2. index => set default
	3. loader => function executed before routing, data isn't accessible bottom up, just deeping throught child route or same level
	4. errorElement => is triggered when some route code throw an error bottom up, if children throw an error on loader function errorElement is triggered
	5. id => useRouteLoadederData() to force route where take data from; because static route have a high priority then dynamic one 
	6. actions => send data
---
```jsx
//app
import { RouterProvider, createBrowserRouter } from 'react-router-dom';

import EditEventPage from './pages/EditEvent';
import ErrorPage from './pages/Error';
import EventDetailPage, {
  loader as eventDetailLoader,
  action as deleteEventAction,
} from './pages/EventDetail';
import EventsPage, { loader as eventsLoader } from './pages/Events';
import EventsRootLayout from './pages/EventsRoot';
import HomePage from './pages/Home';
import NewEventPage from './pages/NewEvent';
import RootLayout from './pages/Root';
import { action as manipulateEventAction } from './components/EventForm';
import NewsletterPage, { action as newsletterAction } from './pages/Newsletter';

const router = createBrowserRouter([
  {
    path: '/',
    element: <RootLayout />,
    errorElement: <ErrorPage />,
    children: [
      { index: true, element: <HomePage /> },
      {
        path: 'events',
        element: <EventsRootLayout />, // overwrite <RootLayout /> and is shared to all children
        children: [
          {
            index: true,
            element: <EventsPage />, // this is what is rendered from outlet
            loader: eventsLoader,
          },
          {
            path: ':eventId',
            id: 'event-detail',
            loader: eventDetailLoader,
            children: [
              {
                index: true,
                element: <EventDetailPage />,
                action: deleteEventAction,
              },
              {
                path: 'edit',
                element: <EditEventPage />,
                action: manipulateEventAction,
              },
            ],
          },
          {
            path: 'new',
            element: <NewEventPage />,
            action: manipulateEventAction,
          },
        ],
      },
      {
        path: 'newsletter',
        element: <NewsletterPage />,
        action: newsletterAction,
      },
    ],
  },
]);

function App() {
  return <RouterProvider router={router} />;
}

export default App;
```