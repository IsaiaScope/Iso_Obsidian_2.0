#### dynamically add class
- template literals are the way
```jsx
return (
  <form onSubmit= {formSubmitHandler}>
    <div className={`form-control ${isValid ? 'invalid' : ''}`}
      <label>Course Goal</label>
      <input type="text" onChange={goalInputChangeHandler} />
    </div>
    <Button type="submit">Add Goal</Button>
  </form>
);
```
---
#### styled components
- [Doc styled components](https://styled-components.com/docs/basics)
```jsx
return (
  <form onSubmit= {formSubmitHandler}>
    <div className={`form-control ${isValid ? 'invalid' : ''}`}
      <label>Course Goal</label>
      <input type="text" onChange={goalInputChangeHandler} />
    </div>
    <Button type="submit">Add Goal</Button>
  </form>
);
```
---