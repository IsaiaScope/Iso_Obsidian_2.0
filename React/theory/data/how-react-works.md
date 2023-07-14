- How Does React Work Behind The Scenes? and Understanding the Virtual DOM & DOM Updates
	- make a snapshot of DOM, when changes are made (by State, Context, props) React create a new snapshot of the DOM, compare with the previous one and modifies the DOM just with necessary, don't recreate the entire of the DOM. React modifies the only parts needed nothing else. 
	- [React.memo(Component)](https://react.dev/reference/react/memo)
		- `prevent unnecessary re-evaluation of component` on DOM
		- check previous props values with actual ones; if nothing change makes React to not re render the component and every child component inside
		- work with `primitive` value => because use === as comparison method; so if props are arrays, objs of functions React.memo re renders component anyway
		- a work around to this problem [useCallBack()](https://react.dev/reference/react/useCallback) hook witch can store a function across component execution
	- useCallBack()
		- N.B. useCallBack() store the function value and it's always the same, sono doesn't update itself automatically when props change, like useEffect() it needs a dependency array
		- be safe from [Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) in JS because a function store the value of variables insides, when a function re evaluate when was saved like a 'snapshot' (as useCallBack works) use saved value and not new ones (as allowToggle in ex down). So add always dependency to useCallBark() for every prop, variables that need to be update for correction execution of the function

- ex React.memo() & useCallBack()
```jsx
// button.js
const Button = (props) => {
  console.log('Button RUNNING');
  return (
    <button
      type={props.type || 'button'}
      className={`${classes.button} ${props.className}`}
      onClick={props.onClick}
      disabled={props.disabled}
    >
      {props.children}
    </button>
  );
};

export default React.memo(Button); // <--

// app.js
import React, { useState, useCallback } from 'react'; // <--

function App() {
  const [showParagraph, setShowParagraph] = useState(false);
  const [allowToggle, setAllowToggle] = useState(false);

  console.log('APP RUNNING');

  const toggleParagraphHandler = useCallback(() => { // <--
    if (allowToggle) {
      setShowParagraph((prevShowParagraph) => !prevShowParagraph);
    }
  }, [allowToggle]);

  const allowToggleHandler = () => {
    setAllowToggle(true);
  };

  return (
    <div className="app">
      <h1>Hi there!</h1>
      <DemoOutput show={showParagraph} />
      <Button onClick={allowToggleHandler}>Allow Toggling</Button>
      <Button onClick={toggleParagraphHandler}>Toggle Paragraph!</Button>
    </div>
  );
}

export default App;
```

---
- Understanding State & State Updates
	- 