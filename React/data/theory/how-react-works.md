- **How Does React Work Behind The Scenes? and Understanding the Virtual DOM & DOM Updates**
	- make a snapshot of DOM, when changes are made (by State, Context, props) React create a new snapshot of the DOM, compare with the previous one and modifies the DOM just with necessary, don't recreate the entire of the DOM. React modifies the only parts needed nothing else. 
	- [React.memo(Component)](https://react.dev/reference/react/memo)
		- `prevent unnecessary re-evaluation of component` on DOM
		- check previous props values with actual ones; if nothing change makes React to not re render the component and every child component inside
		- work with `primitive` value => because use === as comparison method; so if props are arrays, objs of functions React.memo re renders component anyway
		- a work around to this problem [useCallBack()](https://react.dev/reference/react/useCallback) hook witch can store a function across component execution
	- useCallBack()
		- N.B. useCallBack() store the function value and it's always the same, sono doesn't update itself automatically when props change, like useEffect() it needs a dependency array
		- be safe from [Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) in JS because a function store the value of variables insides, when a function re evaluate when was saved like a 'snapshot' (as useCallBack works) use saved value and not new ones (as allowToggle in ex down). So add always dependency to useCallBark() for every prop, variables that need to be update for correction execution of the function
	- [useMemo()](https://react.dev/reference/react/useMemo)
		- is a React Hook that lets you cache the result of a calculation between re-renders, and memorize any kind of data we want
		- need dependency because store data statically
		- useful when hard and long logic is applied to a value prop, to reduce computing weight useMemo() is prefect because you can do: repeat that logic on that input just if the prop change 
---
- **Understanding State & State Updates**
- [[state-react.png]]
- [[scheduled-state-react.png]] => `use always function form with useState()` to be sure to work with latest state, because React give a priority to every state update so sometimes cannot follow the order we except   
- `multiple useState() in the same random function are scheduled together` by React that collect them and executes at 'the same time' and not in two different schedule so you are sure the queue is respected and not other external state can be scheduled between.

---

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

ex. useMemo()
```jsx
// DemoList.js
import React, { useMemo } from 'react'; // <--

const DemoList = (props) => {
  const { items } = props;

  const sortedList = useMemo(() => {
    console.log('Items sorted');
    return items.sort((a, b) => a - b);
  }, [items]); 
  console.log('DemoList RUNNING');

  return (
    <div className={classes.list}>
      <h2>{props.title}</h2>
      <ul>
        {sortedList.map((item) => (
          <li key={item}>{item}</li>
        ))}
      </ul>
    </div>
  );
};

export default React.memo(DemoList);

// App.js
import React, { useState, useCallback, useMemo } from 'react';

function App() {
  const [listTitle, setListTitle] = useState('My List');

  const changeTitleHandler = useCallback(() => {
    setListTitle('New Title');
  }, []);

  const listItems = useMemo(() => [5, 3, 1, 10, 9], []); // <--

  return (
    <div className="app">
      <DemoList title={listTitle} items={listItems} />
      <Button onClick={changeTitleHandler}>Change List Title</Button>
    </div>
  );
}

export default App;
```