|                                          What is an "Effect" (or a "Side Effect")?                                           |
| :--------------------------------------------------------------------------------------------------------------------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------- |
|                                          Main Job: Render Ul & React to User Input                                           | Side Effects: Anything Else                                                                                                                          |
| Evaluate & Render JSX, Manage State & Props, React to (User) Events & Input, Re-evaluate Component upon State & Prop Changes | Store Data in Browser Storage, Send Http Requests to Backend Servers, Set & Manage Timers                                                            |
|    This all is "baked into" React via thc "tools" and features covered in this course (i.e. useState() Hook. Props etc).     | these tasks must happen outside normal component evaluation and render cycle — especially since they might block/delay rendering (e.g. Http request) |

---

1. `useEffect` is a React Hook that lets you synchronize a component with an external system
2. [useEffect](https://react.dev/reference/react/useEffect)
3. useEffect(setup, dependencies?)
4. ex

```jsx
import { useEffect } from "react";
import { createConnection } from "./chat.js";

function ChatRoom({ roomId }) {
	const [serverUrl, setServerUrl] = useState("https://localhost:1234");

	useEffect(() => {
		const connection = createConnection(serverUrl, roomId);
		connection.connect();
		return () => {
			connection.disconnect(); // <-- cleanup function
		};
	}, [serverUrl, roomId]);
	// ...
}
```

**Let’s illustrate this sequence for the example above.**
When the `ChatRoom` component above gets added to the page, it will connect to the chat room with the initial `serverUrl` and `roomId`. If either `serverUrl` or `roomId` change as a result of a re-render (say, if the user picks a different chat room in a dropdown), your Effect will _disconnect from the previous room, and connect to the next one._ When the `ChatRoom` component is removed from the page, your Effect will disconnect one last time.

---

**_N.B._** You need to pass two arguments to `useEffect`:

1. A *setup function* with setup code that connects to that system.
   - It should return a *cleanup function* with cleanup code that disconnects from that system.
2. A list of dependencies including every value from your component used inside of those functions.
   **React calls your setup and cleanup functions whenever it’s necessary, which may happen multiple times:**
3. Your setup code runs when your component is added to the page *(mounts)*.
4. After every re-render of your component where the dependencies have changed:
   - First, your cleanup code runs with the old props and state.
   - Then, your setup code runs with the new props and state.
5. Your cleanup code runs one final time after your component is removed from the page *(unmounts).*
6. ex `NOT USE THIS CODE BEACUSE TOO MANY STATE USED EVEN WITHOUT FUNCTION, SO IN RARE CASE CANNOT WORK PROPERLY` => USE USEREDUCER(), following example is just to show how **_cleanup function_** works

```jsx
import React, { useState, useEffect } from "react";

import Card from "../UI/Card/Card";
import classes from "./Login.module.css";
import Button from "../UI/Button/Button";

const Login = (props) => {
	const [enteredEmail, setEnteredEmail] = useState("");
	const [emailIsValid, setEmailIsValid] = useState();
	const [enteredPassword, setEnteredPassword] = useState("");
	const [passwordIsValid, setPasswordIsValid] = useState();
	const [formIsValid, setFormIsValid] = useState(false);

	useEffect(() => {
		// <--
		console.log("EFFECT RUNNING");

		return () => {
			console.log("EFFECT CLEANUP");
		};
	}, []);

	useEffect(() => {
		// <--
		const identifier = setTimeout(() => {
			console.log("Checking form validity!");
			setFormIsValid(
				enteredEmail.includes("@") && enteredPassword.trim().length > 6
			);
		}, 500);

		return () => {
			// <--  cleanup function
			console.log("CLEANUP");
			clearTimeout(identifier);
		};
	}, [enteredEmail, enteredPassword]);

	const emailChangeHandler = (event) => {
		setEnteredEmail(event.target.value);
	};

	const passwordChangeHandler = (event) => {
		setEnteredPassword(event.target.value);
	};

	const validateEmailHandler = () => {
		setEmailIsValid(enteredEmail.includes("@"));
	};

	const validatePasswordHandler = () => {
		setPasswordIsValid(enteredPassword.trim().length > 6);
	};

	const submitHandler = (event) => {
		event.preventDefault();
		props.onLogin(enteredEmail, enteredPassword);
	};

	return (
		<Card className={classes.login}>
			<form onSubmit={submitHandler}>
				<div
					className={`${classes.control} ${
						emailIsValid === false ? classes.invalid : ""
					}`}
				>
					<label htmlFor="email">E-Mail</label>
					<input
						type="email"
						id="email"
						value={enteredEmail}
						onChange={emailChangeHandler}
						onBlur={validateEmailHandler}
					/>
				</div>
				<div
					className={`${classes.control} ${
						passwordIsValid === false ? classes.invalid : ""
					}`}
				>
					<label htmlFor="password">Password</label>
					<input
						type="password"
						id="password"
						value={enteredPassword}
						onChange={passwordChangeHandler}
						onBlur={validatePasswordHandler}
					/>
				</div>
				<div className={classes.actions}>
					<Button type="submit" className={classes.btn} disabled={!formIsValid}>
						Login
					</Button>
				</div>
			</form>
		</Card>
	);
};

export default Login;
```

---
