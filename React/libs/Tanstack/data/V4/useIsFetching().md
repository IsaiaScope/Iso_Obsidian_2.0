- [DOC of useIsFetching()](https://tanstack.com/query/v4/docs/react/reference/useIsFetching)
- _useIsFetching_
  - In smaller apps
  - used isFetching from useQuery return object
  - Reminder: isLoading is isFetching plus no cached data
  - In a larger app
  - Loading spinner whenever any query isFetching
  - useIsFetching tells us this!
  - No need for isFetching on every custom hook / useQuery call

---