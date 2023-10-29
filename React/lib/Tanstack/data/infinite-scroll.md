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
