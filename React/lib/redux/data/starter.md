- [Redux](https://redux.js.org/introduction/getting-started)
- [[state-redux.png]]
- [[how-redux-works.png]]
-  useSelector() hook subscribe and unsubscribe automatically so update the component when data change this hook return a part of the state, using the following syntax 
```jsx
const counter = useSelector(state => state.counter)
```
- Modern Approach Toolkit
	- `configureStore` sets up a well-configured Redux store with a single function call, including combining reducers, adding the thunk middleware, and setting up the Redux DevTools integration. It also is easier to configure than `createStore`, because it takes named options parameters.
	- `createSlice` lets you write reducers that use [the Immer library](https://immerjs.github.io/immer/) to enable writing immutable updates using "mutating" JS syntax like `state.value = 123`, with no spreads needed. It also automatically generates action creator functions for each reducer, and generates action type strings internally based on your reducer's names. Finally, it works great with TypeScript.
	- [[redux-side-effect.png]]
	```
	
```
