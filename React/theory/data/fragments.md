`<Fragment>`, often used via `<>...</>` syntax, lets you group elements without a wrapper node.
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