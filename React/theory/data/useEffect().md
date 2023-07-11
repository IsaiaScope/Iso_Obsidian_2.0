


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
		connection.disconnect();  
	};  
}, [serverUrl, roomId]);  
// ...  
}
```








