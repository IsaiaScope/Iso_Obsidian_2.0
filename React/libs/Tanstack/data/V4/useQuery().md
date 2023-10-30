- [DOC of UseQuery()](https://tanstack.com/query/latest/docs/react/reference/useQuery)

---

_is Fetching vs. is Loading_

- is Fetching
  - the async query function hasn't yet resolved
- isLoading
  - no cached data, plus isFetching
- Why does it matter if the data is stale?
- Data refetch only triggers for stale data
- For example, component remount, window refocus
- staleTime translates to "max age"
- How to tolerate data potentially being out of date?

---

_staleTime vs. cacheTime_

- staleTime is for re-fetching
- Cache is for data that might be re-used later
- query goes into "cold storage" if there's no active useQuery
- cache data expires after cacheTime (default: five minutes)
- how long it's been since the last active useQuery
  -After the cache expires, the data is garbage collected
- Cache is backup data to display while fetching

---

- [DOC of Error Handling](https://tanstack.com/query/latest/docs/react/guides/query-retries?from=reactQueryV3&original=https%3A%2F%2Ftanstack.com%2Fquery%2Fv3%2Fdocs%2Fguides%2Fquery-retries)

- Account for error, loading and results
- Be sure to choose a different query key! (not posts)
- React Query uses the key for cache / stale time
- query function needs post Id parameter
- () => fetchComments (post. id)

---

_Why don't comments refresh?_

- Every query uses the same key (comments)
- Data for queries with known keys only refetched upon trigger
- Example triggers:
  - component remount
  - window refocus
  - running refetch function
  - automated refetch
  - query invalidation after a mutation

_Solution?_

- Option: remove programmatically for every new title
  - it's not easy
  - it's not really what we want
- No reason to remove data from the cache
  - we're not even performing the same query!
- Query includes post ID
  - Cache on a per-query basis
  - don't share cache for any "comments" query regardless of post id
- What we really want: label the query for each post separately
- Pass array for the query key, not just a string
- Treat the query key as a dependency array
  - When key changes, create a new query
- Query function values should be part of the key
  - ['comments', post.id]

```jsx
const { data, isLoading, isError, error } = useQuery(
	["comments", post.id],
	() => fetchComments(post.id)
);
```
