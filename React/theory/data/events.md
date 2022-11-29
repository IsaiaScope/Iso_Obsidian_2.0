#### Events
1. ***do not execute the handler function (like clickHandler) in templates*** because that code will be executed when the component is compiled and we want just to point that function and react takes care when to call it in base of event handled 


````ad-example
title:  *Ex: click event*

```jsx
// click event
const clickHandler = () => { console. log('Clicked!!!!'); }
return (
	<button onClick={clickHandler}>Change Title</button>
)
```

````