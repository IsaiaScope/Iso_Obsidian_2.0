#### Render a list
1. React allow to use tag components directly in render method combine with map. *Differently from Angular you can use function directly in template*.
2.  `NOTE` *key value in a list*: for improve performance adding a unique value on each element of the array because React in this way know witch element has to re render and avoid loading the entire of the list again 
```jsx
return (
  <div>
    {props.items.map(expense => {
	    <ExpenseItem 
		    key={expense.id}
		    title={expense.title}
		  />
	  }
  </div>
)
```
---
#### Render content conditionally
1.  Is possible to use ternary condition or other js logic (like &&, ||) directly in 'template part' of component
2. `let expensesContent = <p>No expenses found.</p>;`is a valid jsx expression
3. 
```jsx
let expensesContent = <p>No expenses found.</p>;

if (props.items.length > 0) {
	expensesContent = filteredExpenses.map((expense) => {
		return <ExpenseItem
		 key={expense.id}
		 title={expense.title}
		 amount={expense.amount}
		 date={expense.date} 
		/>
	}
}
render(
	{expensesContent}
)
```
---
#### return render() conditionally
1.  Is possible to use ternary condition or other js logic (like &&, ||) directly in 'template part' of component
2. 
```jsx
const ExpensesList = (props) => {
  let expensesContent = <p>No expenses found. </p>;
   if (props.items.length === 0) {
     return <h2 className"expenses-list_fallback">Ciao</h2>;
   return (
    <ul className='expenses-list'>
       {props.items.map((expense) => (
        <ExpenseItem
          key={expense.id}
          title={expense.title}
          amount={expense.amount}
          date={expense.date}
        />
        ))}
    </ul>

```
---
