#### useState()
1. useState is native React function to manage status change.
2. u can't use let variables locally to update components; `NOTE` *there is an important concept called State that says to React component to update the situation, re render and execute again all component logic, so also the logic, external the method that contain the useState() function, is called again*
3. in the fallowing snippet *console.log('execute again when setTitle() is called')* is called every time setTitle() is called.
````ad-example
title: *useState() snippet*
collapse: closed
```jsx
const Components = (props) => {
 const [title, setTitle] = useState(props.title);
 const clickHandler = () => setTitle('Updated!');
 console.log('execute again when setTitle is called')
 return (
	 <h1>{title}</h1>
	 <button onClick={clickHandler}>Change Title</button>
 )
}
```
````
---

1. se also [click event](events#^7ffdf5)