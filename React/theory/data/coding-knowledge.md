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