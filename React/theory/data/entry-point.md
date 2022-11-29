#### React
1. With React, you build a component tree with one root component that's
mounted into a DOM node. It means that you have a root node which then has more components nested beneath it.
2. React is ***declarative***. What makes it declarative is that we describe a final UI for any given state representation. It is the opposite of providing manual DOM mutations to transition between UI states (imperative).
3. ***import React from 'react'*** isn't necessary anymore because in closest updates react do that under the hood. The fallowing code explain what happen behind the scenes and how u must write code in past days. Now we have jsx syntax like in the second block code and that is mush more readable 
```jsx
// before
return React. createElement(
   'div',
  React.createElement('h2', {}, "Let's get started!"),
  React.createElement(Expenses, { items: expenses })
)
// now
return (
  <div>
     <h2>Let's get started!</h2>
     <Expenses items=(expenses) />
  </div>
)
```
---



- [Array.prototype.flat()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)
