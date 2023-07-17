- Outsource stateful logic into re-usable functions
- Unlike "regular functions". custom hooks can use other React hooks and React state
- create hooks folder
- [custom-hooks](https://react.dev/learn/reusing-logic-with-custom-hooks)
- basic ex
```jsx
// custom-hook.js
import { useState, useEffect } from 'react';

const useCounter = (forwards = true) => {
  const [counter, setCounter] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      if (forwards) {
        setCounter((prevCounter) => prevCounter + 1);
      } else {
        setCounter((prevCounter) => prevCounter - 1);
      }
    }, 1000);

    return () => clearInterval(interval);
  }, [forwards]);

  return counter;
};

export default useCounter;


// BackwardCounter.js
const BackwardCounter = () => {
  const counter = useCounter(false);

  return <Card>{counter}</Card>;
};

export default BackwardCounter;
```