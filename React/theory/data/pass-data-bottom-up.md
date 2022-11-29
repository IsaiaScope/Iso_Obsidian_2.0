#### Child-to-Parent Component Communication (Bottom-up)
1. First step create the prop to pass child component, and the logic is located in child component that execute the function passed
2. in the following example is *onSaveExpenseData={saveExpenseDataHandler}* 
3. 
```jsx
const NewExpense = () => {
const saveExpenseDataHandler = (enteredExpenseData) => {
   const expenseData = {
     ...enteredExpenseData,
     id: Math.random().toString()
  };
  console.log(expenseData);
 return (
   <div className='new-expense'>
     <ExpenseForm onSaveExpenseData={saveExpenseDataHandler} />
  </div>
);
export default NewExpense;
```

