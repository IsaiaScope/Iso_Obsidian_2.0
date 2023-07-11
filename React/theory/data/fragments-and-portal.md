1. `<Fragment>`, often used via `<>...</>` syntax, lets you group elements without a wrapper node.
```jsx
<>  
	<OneChild />  
	<AnotherChild />  
</>
```
```jsx
<React.Fragment>  
	<OneChild />  
	<AnotherChild />  
<React.Fragment/>
```
```jsx
const Wrapper = props => {
  return props.children;
};

export default Wrapper;
```
```jsx
<Fragment>
	<AddUser onAddUser={addUserHandler} />
	<UsersList users={usersList} />
</Fragment>
```
---
2. ***portal***
	1. `createPortal` lets you render some children into a different part of the DOM. not just in the root element dispose by default from React
	2. [portal](https://react.dev/reference/react-dom/createPortal)
	3. `createPortal(children, domNode, key?)`
	4. add to index html the portal element => Best practice
```jsx
// create modal and use a portal to mount it in a specific DOM point const 
import { createPortal } from 'react-dom';  

// ... index.html
	<div id="backdrop-root"></div>
	<div id="overlay-root"></div>
	<div id="root"></div>
```

```jsx
const Backdrop = (props) => {
  return <div className={classes.backdrop} onClick={props.onConfirm} />;
};

const ModalOverlay = (props) => {
  return (
    <Card className={classes.modal}>
      <header className={classes.header}>
        <h2>{props.title}</h2>
      </header>
      <div className={classes.content}>
        <p>{props.message}</p>
      </div>
      <footer className={classes.actions}>
        <Button onClick={props.onConfirm}>Okay</Button>
      </footer>
    </Card>
  );
};

const ErrorModal = (props) => {
  return (
    <React.Fragment>
      {ReactDOM.createPortal(
        <Backdrop onConfirm={props.onConfirm} />,
        document.getElementById('backdrop-root')
      )}
      {ReactDOM.createPortal(
        <ModalOverlay
          title={props.title}
          message={props.message}
          onConfirm={props.onConfirm}
        />,
        document.getElementById('overlay-root')
      )}
    </React.Fragment>
  );
};

export default ErrorModal;
```
```jsx
// called 
<ErrorModal
	title={error.title}
	message={error.message}
	onConfirm={errorHandler}
/>
```