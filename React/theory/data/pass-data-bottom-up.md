#### Child-to-Parent Component Communication (Bottom-up)
1. *Do not execute the handler function (like clickHandler) in templates* because that code will be executed when the component is compiled and we want just to point that function and react takes care when to call it in base of event handled.
2. [Events doc](https://reactjs.org/docs/events.html)

````ad-example
title: *click event snippet*
collapse: closed
```jsx
const clickHandler = () => { console.log('Clicked!!!!') }
return (
	<button onClick={clickHandler}>Change Title</button>
)
```
````
