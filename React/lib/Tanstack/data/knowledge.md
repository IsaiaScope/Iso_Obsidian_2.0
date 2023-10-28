_USEQUERY_

_is Fetching vs. is Loading_
- is Fetching
	- the async query function hasn't yet resolved
- isLoading
	- no cached data, plus isFetching

• Why does it matter if the data is stale? 
	• Data refetch only triggers for stale data 
	• For example, component remount, window refocus • staleTime translates to "max age" 
	• How to tolerate data potentially being out of date?

staleTime vs. cacheTime 
• staleTime is for re-fetching 
• Cache is for data that might be re-used later 
• query goes into "cold storage" if there's no active useQuery 
• cache data expires after cacheTime (default: five minutes) 
• how long it's been since the last active useQuery 
•After the cache expires, the data is garbage collected 
• Cache is backup data to display while fetching

- [DOC of  Error Handling](https://tanstack.com/query/latest/docs/react/guides/query-retries?from=reactQueryV3&original=https%3A%2F%2Ftanstack.com%2Fquery%2Fv3%2Fdocs%2Fguides%2Fquery-retries)

---