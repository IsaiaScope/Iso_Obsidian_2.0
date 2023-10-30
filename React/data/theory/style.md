#### Dynamically add classes
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
#### Using: styled components
- [Doc styled components](https://styled-components.com/docs/basics)
---
#### Using: CSS modules
- [CSS Module](https://create-react-app.dev/docs/adding-a-css-modules-stylesheet/)
---