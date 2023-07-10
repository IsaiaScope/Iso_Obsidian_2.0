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
```jsx
// create modal and use a portal to mount it in a specific DOM point const Modal = (props) => { 
	return ( 
		<React.Fragment> {ReactDOM.createPortal( 
			<div>This is the modal</div>,
			document.getElementById('modal-root'), 
		)} 
		</React.Fragment>
	) 
}
```