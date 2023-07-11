


|What is an "Effect" (or a "Side Effect")? |
:----------------:|:-------------|
| Main Job: Render Ul & React to User Input  |  Side Effects: Anything Else  |
| 	Evaluate & Render JSX,	Manage State & Props,	React to (User) Events & Input,	Re-evaluate Component upon State &	Prop Changes  | Store Data in Browser Storage, Send Http Requests to Backend Servers, Set & Manage Timers |
| This all is "baked into" React via thc "tools" and features covered in this course (i.e. useState() Hook. Props etc).  |  these tasks must happen outside normal component evaluation and render cycle — especially since they might block/delay rendering (e.g. Http request) |

---
1. `useEffect` is a React Hook that lets you synchronize a component with an external system
2. [useEffect](https://react.dev/reference/react/useEffect)
3. useEffect(setup, dependencies?)
4.  ex
```jsx
import { useEffect } from 'react';  
import { createConnection } from './chat.js';  

function ChatRoom({ roomId }) {  
const [serverUrl, setServerUrl] = useState('https://localhost:1234');  

useEffect(() => {  
	const connection = createConnection(serverUrl, roomId);  
	connection.connect();  
	return () => {  
		connection.disconnect(); // <-- cleanup function
	};  
}, [serverUrl, roomId]);  
// ...  
}
```
**Let’s illustrate this sequence for the example above.**
When the `ChatRoom` component above gets added to the page, it will connect to the chat room with the initial `serverUrl` and `roomId`. If either `serverUrl` or `roomId` change as a result of a re-render (say, if the user picks a different chat room in a dropdown), your Effect will _disconnect from the previous room, and connect to the next one._ When the `ChatRoom` component is removed from the page, your Effect will disconnect one last time.

---
***N.B.*** You need to pass two arguments to `useEffect`:
1. A _setup function_ with setup code that connects to that system.
    - It should return a _cleanup function_ with cleanup code that disconnects from that system.
2. A list of dependencies including every value from your component used inside of those functions.
**React calls your setup and cleanup functions whenever it’s necessary, which may happen multiple times:**
1. Your setup code runs when your component is added to the page _(mounts)_.
2. After every re-render of your component where the dependencies have changed:
    - First, your cleanup code runs with the old props and state.
    - Then, your setup code runs with the new props and state.
3. Your cleanup code runs one final time after your component is removed from the page _(unmounts)._








