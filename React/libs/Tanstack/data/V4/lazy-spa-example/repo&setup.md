- [GitHub Repo of lazy-days-spa](https://github.com/bonnie/udemy-REACT-QUERY/tree/react-query-v4/completed-apps/lazy-days-spa)

---

- _Caveats about Lazy Days App_

  - Not production ready!
    - not responsive
    - not mobile friendly
    - appointment updates aren't auth-protected
    - no admin user / interface to update staff, treatments
  - Client and server
    - download both and run npm install
    - create env file in server and add EXPRESS_SECRET !

- _React Query in Larger App_
  - centralizing fetching indicator / error handling
  - refetching data
  - integrating with auth
  - dependent queries
  - testing
  - more examples of useQuery, mutations, pagination, prefetching

---

- _Check Theory Behind The Example_

  - Wrap React Query in a Custom Hoom
  - useIsFetching()
  - Handling Errors Globally with Chakra UI Toas
  - Adding Data to the Cache
  - [[cache-methods.png | Cache methods]]

- _Prefetch Treatments _

  - saw prefetch with pagination
    - prefetch next page
  - different trigger: prefetch treatments on home page load
    - user research: 85% of home page loads are followed by treatments tab loads
    - Treatments don't change often, so cached data isn't really a problem
  - garbage collected if no useQuery is called after cacheTime
    - if typically not loaded by default cacheTime (5 minutes), specify longer cache Time
  - prefetchQuery is a method on the queryClient
    - adding to the client cache
  - useQueryClient returns queryClient (within Provider)
  - Make a usePrefetchTreatments hook within useTreatments. ts
    - uses the same query function and key as the useTreatments call
    - call usePrefetchTreatments from Home component
    - [[prefetch&cache-flow.png | Cache Flow]]

- _Why doesn't new data load?_
  - using the same key for every query
  - after clicking arrow to load new month
    - data is stale (staleTime = 0), but...
    - nothing to trigger refetch
      - component remount
      - window refocus
      - running refetch function manually
      - automated refetch
- Only fetch new data for known key for the above reasons
- _Solution?_ New key for each month

  - treat keys as dependency arrays!

- _Summary_
  - Pre-populating data options: -
    - pre-fetch, setQueryData, placeholderData, initialData
  - Pre-fetch to pre-populate cache
    - on component render
    - on page (month/year) update
    - keepPreviousData only useful if background doesn't change
  - Treat keys as dependency arrays

---

- _Filtering with the select option_
  - Allow user to filter out any appointments that aren't available
  - Why is the select option the best way to do this?
    - React Query memo-izes to reduce unnecessary computation
    - tech details:
      - triple equals comparison of select function
      - only runs if data changes and the function has changed
    - need a stable function (useCallback for anonymous function)
    - reference: https://tkdodo.eu/blog/react-query-data-transformations
  - State contained in hook (like monthYear)
  - Filter function in utils: getAvailableAppointments
- _Selector for useStaff_
  - Activate radio buttons on AllStaff page for specific treatments
  - State tracked in useStaff (returns getter/setter)
  - Selector function pre-written in utils.js: filterByTreatment
    - need to make select callback into anonymous function (needs filter parameter)
    - (unfilteredStaff) = filterByTreatment (unfilteredStaff, filter)
    - Wrap in useCallback for caching benefits
  - If al1 is selected, don't use a filter function
    - like showAll = true for useAppointments
- _Re-fetching! Why? When?_
  - Re-fetch ensures stale data gets updated from server
    - Seen when we leave the page and refocus
  - Stale queries are re-fetched automatically in the background when:
    - New instances of the query mount
    - Every time a react component (that has a useQuery call) mounts
    - The window is refocused - The network is reconnected
    - configured refetchInterval has expired
      - Automatic polling
- _Re-fetching! How?_
  - Control with global or query-specific options:
    - refetchOnMount, refetchOnWindowFocus, refetchOnReconnect, refetchInterval
  - Or, imperatively: refetch function in useQuery return object
  - reference: https://react-query.tanstack.com/guides/important-defaults
- _Suppressing Re-Fetch_
  - How?
    - Increase stale time
    - turn off refetchOnMount / refetchOnWindowFocus / refetchOnReconnect
  - Only for very rarely changed, not mission-critical data
    - treatments or staff (definitely not appointments!)
- _Update Global Settings_
  - Global default options vs individual query options
  - Here, want settings for everything but appointments
    - User profile and user appointments invalidated after mutations
    - Appointments get special settings (including auto-refetching on interval)
  - Global options in src/react-query/queryClient.ts
- _Polling / Auto Re-Fetching_
  - Appointments is the opposite of treatments and staff
    - We want it to be updated even if the user doesn't take action
    - Appointments may change on server, want to keep up-to-date
  - Overrode defaults for staleTime, cacheTime, refetchOn\*
  - Use refetchInterval option to useQuery
    - Reference: https://react-query.tanstack.com/examples/auto-refetching
  - What about userAppointments?
    - Can this go with the defaults?
    - Yes, because it will never be updated "from underneath us"
    - Client will know if there are any changes to logged-in user appointments
    - It's only other appointments that might change on the server
- _Summary_
  - select option for filtering
    - stable function to take advantage of caching
  - Suppressing re-fetch with options
  - Polling / re-fetching at intervals

---

```tsx
/* eslint-disable no-console */
import { createStandaloneToast } from '@chakra-ui/react';
import { QueryClient } from '@tanstack/react-query';

const toast = createStandaloneToast();

function queryErrorHandler(error: unknown): void {
  // error is type unknown because in js, anything can be an error (e.g. throw(5))
  const title =
    error instanceof Error ? error.message : 'error connecting to server';

  // prevent duplicate toasts
  toast.closeAll();
  toast({ title, status: 'error', variant: 'subtle', isClosable: true });
}

export function generateQueryClient(): QueryClient {
  return new QueryClient({
    // from https://tanstack.com/query/v4/docs/guides/testing#turn-off-network-error-logging
    logger: {
      log: console.log,
      warn: console.warn,
      // ✅ no more errors on the console for tests
      // eslint-disable-next-line @typescript-eslint/no-empty-function
      error: process.env.NODE_ENV === 'test' ? () => {} : console.error,
    },
    defaultOptions: {
      queries: {
        onError: queryErrorHandler,
        staleTime: 600000, // 10 minutes
        cacheTime: 900000, // default cacheTime is 5 minutes; doesn't make sense for staleTime to exceed cacheTime
        refetchOnMount: false,
        refetchOnWindowFocus: false,
        refetchOnReconnect: false,
        retry: !(process.env.NODE_ENV === 'test'), // set retry to false for tests
      },
      mutations: {
        onError: queryErrorHandler,
      },
    },
  });
}

export const queryClient = generateQueryClient();
```

---

- _NOTE No more call if the same one is already running_
- _Remove userAppointments Query_
  - Make sure user appointments data is cleared on sign out
  - queryClient. removeQueries
  - Why not use removeQueries for user data?
    - setQueryData invokes onSuccess (removeQueries does not )
    - userAppointments does not need onSuccess for useUser
- _Summary Auth_
  - useQuery caches user data and refreshes from server
    - refreshing from server will be important for mutations
  - useUser manages user data in query cache and localStorage
    - set query cache using setQueryData on signin / signout
    - onSuccess callback manages localStorage
  - user appointments query dependent on user state
    - Remove data on signout with removeQueries
