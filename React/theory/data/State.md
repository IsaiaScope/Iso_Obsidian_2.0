#### useState()
1. useState is native React function to manage status change.
2. You can't use let variables locally to update components; `NOTE` *there is an important concept called State that says to React component to update the situation, re render and execute again all component logic, so also the logic, external the method that contain the useState() function, is called again*
3. In the fallowing snippet *console.log('execute again when setTitle() is called')* is called every time setTitle() is called.
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
4. Multiple state management could be done in different ways someone better than other in different situation

````ad-example
title: *multiple state management snippet*
collapse: closed
```jsx
const Components = (props) => {

const [enteredTitle, setEnteredTitle] = useState('');
const [enteredAmount, setEnteredAmount] = useState('');
const [enteredDate, setEnteredDate] = useState('');

const titleChangeHandler = (event) => {
	setEnteredTitle(event.target.value);
};

const amountChangeHandler = (event) => {
	setEnteredAmount(event.target.value);
};

const dateChangeHandler = (event) => {
	setEnteredDate(event.target.value);
};

const submitHandler = (event) => {
	event.preventDefault();
	const expenseData = {
		title: enteredTitle,
		amount: enteredAmount,
		date: new Date(enteredDate)
	};
};

```
````