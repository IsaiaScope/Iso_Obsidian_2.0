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
