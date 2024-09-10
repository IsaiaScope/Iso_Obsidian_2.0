#### useState()

1. useState is native React function to manage status change.
2. You can't use let variables locally to update components; `NOTE` _there is an important concept called State that says to React component to update the situation, re render and execute again all component logic, so also the logic, external the method that contain the useState() function, is called again_
3. In the fallowing snippet _console.log('execute again when setTitle() is called')_ is called every time setTitle() is called.
4. Parameters: _when state depends on previous state u must use a function; viceversa u can use just a value_

````ad-example
title: *useState()*
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

````ad-important
title: *useState() when new state depends on previous state*
collapse: closed
```jsx
const [expenses, setExpenses] = useState(DUMMY_ARRAY);
const addExpenseHandler = (expense) => {
	setExpenses((prevExpenses) => {
		return [expense, ...prevExpenses];
	}
};

```
````

---

5. _Multiple state management_ could be done in different ways someone better than other in different situation, using a single useState() or multiple

````ad-example
title: *multiple useState()*
collapse: closed
```jsx
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
}
```
````

````ad-success
title: *single useState() `RIGHT`*
collapse: closed
pass a function to state, the function has as argument the last state and so you are sure to done things correctly
```jsx
const [userInput, setUserInput] = useState({
	enteredTitle: '',
	enteredAmount: '',
	enteredDate: '',
});

const titleChangeHandler = (event) => {
	setUserInput((prevState) => {
		return { ...prevState, enteredTitle: event.target.value };
	});
};
const amountChangeHandler = (event) => {
	setUserInput((prevState) => {
		return { ...prevState, enteredAmount: event.target.value };
	});
};
const dateChangeHandler = (event) => {
	setUserInput((prevState) => {
		return { ...prevState, enteredDate: event.target.value };
	});
};
```
````

````ad-warning
title: *single useState() `ERROR`*
collapse: closed
this approach is wrong because you can't be sure in that moment the previous state is the right one, React manage the status change put every operation in a queue but the order you could be different of what you think and mess up things
```jsx
const [userInput, setUserInput] = useState({
	enteredTitle: '',
	enteredAmount: '',
	enteredDate: '',
});
const titleChangeHandler = (event) => {
	setUserInput({
	...userInput,
	enteredTitle: event.target.value,
	});
};
const amountChangeHandler = (event) => {
	setUserInput({
	...userInput,
	enteredAmount: event.target.value,
	});
};
const dateChangeHandler = (event) => {
	setUserInput({
	...userInput,
	enteredDate: event.target.value,
	});
};
```
````

---

6.  **_Computed value / Derived State_**
    when a variable depends on state value, because useState rerender the component also al the code inside the component function is executed so any variable could be reassign and modify by some logic

```jsx
const Comp = (props) => {
	const [var, func] = useState('2020');
	let var2 = 'some value'

	if(var === '2021') {
		var2 = 'ex 1'
	}
	if(var === '2022') {
		var2 = 'ex 2'
	}
	if(var === '2023') {
		var2 = 'ex 3'
	}

	const changeYearHandle = (year) => {
		func(year);
	}
}
```

---
