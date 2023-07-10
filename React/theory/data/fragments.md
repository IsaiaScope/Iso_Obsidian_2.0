1. `<Fragment>`, often used via `<>...</>` syntax, lets you group elements without a wrapper node.
```jsx
<>  
	<OneChild />  
	<AnotherChild />  
</>
```
```jsx
<React.Fragment>  
	<OneChild />  
	<AnotherChild />  
<React.Fragment/>
```
---
2. portal
	1. `createPortal` lets you render some children into a different part of the DOM.
	2. [portal](https://react.dev/reference/react-dom/createPortal)
	3. `createPortal(children, domNode, key?)`
```jsx
// create modal and use a portal to mount it in a specific DOM point const 
import { createPortal } from 'react-dom';  

// ...  
<>  
	<p>This child is placed in the parent div.</p>  
	{createPortal(  
		<p>This child is placed in the document body.</p>,  
		document.getElementById('modal-root'), 
	)}  
</>
```