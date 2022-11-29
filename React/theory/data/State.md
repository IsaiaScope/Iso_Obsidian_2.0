#### State
1. do not execute the clickHandler intemplate because that code will be executed when the component is compiled and we want just to point that function and react takes care when to call it in base of event handled 
```jsx
// click event
const clickHandler = ( ) => { console. log('Clicked!!!!'); }
return (
	<button onClick={clickHandler}>Change Title</button>
)
```

