1. Sometimes, you have more complex state — for example if it got multiple states. multiple ways of changing it or dependencies to other states
2. useState() then often becomes hard or error-prone to use — it's easy to write bad, inefficient or buggy code in such scenarios
3. useReducer() can be used as a replacement for useState() if you need "more powerful state management"
```jsx
const [state, dispatch] = useReducer(reducer, initialArg, init?)
```
Parameters
- `reducer`: The reducer function that specifies how the state gets updated. It must be pure, should take the state and action as arguments, and should return the next state. State and action can be of any types.
- `initialArg`: The value from which the initial state is calculated. It can be a value of any type. How the initial state is calculated from it depends on the next `init` argument.
- **optional** `init`: The initializer function that should return the initial state. If it’s not specified, the initial state is set to `initialArg`. Otherwise, the initial state is set to the result of calling `init(initialArg)`.
Returns 
`useReducer` returns an array with exactly two values:
1. The current state. During the first render, it’s set to `init(initialArg)` or `initialArg` (if there’s no `init`).
2. The `dispatch` that lets you update the state to a different value and trigger a re-render.