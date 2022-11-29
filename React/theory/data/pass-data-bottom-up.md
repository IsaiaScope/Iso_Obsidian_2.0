#### Child-to-Parent Component Communication (Bottom-up)
1. First step create the prop to pass child component, and the logic is located in parent component but the method is called in child component
2. in the following example is *onSaveExpenseData={saveExpenseDataHandler}* 
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

