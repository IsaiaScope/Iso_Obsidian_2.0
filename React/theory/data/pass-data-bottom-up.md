#### Child-to-Parent Component Communication (Bottom-up)
1. You can accept a function via props and call it from inside the lower-level (child) component to then trigger some action in the parent component (which passed the function).
2. First step create the prop to pass child component, and the logic is located in parent component but the method is called in child component
3. in the following example is *onSaveExpenseData={saveExpenseDataHandler}* 
```jsx
// parent
const NewExpense = () => {
	const saveExpenseDataHandler = (data) => {
		console.log(data);
	};
	return ( 
		 <ExpenseForm onSaveExpenseData={saveExpenseDataHandler} />
	);
export default NewExpense;
}

// child
const ExpenseForm = (props) => {	
	const clickHandler = () => { 
		props.onSaveExpenseData(expenseData);
	}
	return (
		<button onClick={clickHandler}>Change Title</button>
	)
export default ExpenseForm;
```

