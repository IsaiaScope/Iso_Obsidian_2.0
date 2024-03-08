---
tags:
---

# Init

Whenever we run our test, they are being executed in a Node.js environment.
There is no browser like Chrome or Firefox involved.
Whenever we render our component by calling that render function, a fake browser environment is being
created by a library called JS Dom.

Our component is rendered, HTML is taken from it and it's placed into this kind of fake browser environment.
So we can imagine that there kind of somewhat is real HTML to it we are working with here.
After we render our component, we can then access elements that have been placed or rendered in here
by using this screen object.
And we imported that screen object from the React Testing library.

![[Pasted image 20240308072647.png]]
![[Pasted image 20240308073040.png]]
![[Pasted image 20240308073131.png]]

Role

Now, this entire role system is the primary or the preferred way of finding elements that have been

rendered by our component React Testing library really pushes you in this direction.

They want you to try to find elements based upon role.
![[Pasted image 20240308073427.png]]

Expect

Whenever we want to make an assertion, we're going to use the expect function.

Expect is provided by the just testing framework.

It is a global variable, so that means we do not need to import it or anything like that.

Whenever we call expect, we are always going to pass in some value right here.

We're then going to chain on a function that we refer to as a matcher.

A matcher is going to take a look at the value we passed in and make sure that some property or attribute

of it is equal to maybe something we provide or just make sure that the value we provided is present

in the document or exists or any of a variety of different checks.

There are many different matters available to us.

![[Pasted image 20240308074024.png]]
![[Pasted image 20240308074039.png]]

All of these matters are generally concerned with making sure that some DOM element has some particular

attribute or is present, has text and so on.

user

![[Pasted image 20240308074716.png]]

```jsx
import { useState } from "react";

function UserForm({ onUserAdd }) {
	const [email, setEmail] = useState("");
	const [name, setName] = useState("");

	const handleSubmit = (event) => {
		event.preventDefault();
		onUserAdd({ name, email });
		setEmail("");
		setName("");
	};

	return (
		<form onSubmit={handleSubmit}>
			<div>
				<label htmlFor="name">Name</label>
				<input
					id="name"
					value={name}
					onChange={(e) => setName(e.target.value)}
				/>   
			</div>
			<div>
				<label htmlFor="email">Enter Email</label>
				<input
					id="email"
					value={email}
					onChange={(e) => setEmail(e.target.value)}
				/>
			</div> <button>Add User</button> 
		</form>
	);
}

export default UserForm;
```

```js
import { render, screen } from "@testing-library/react";
import user from "@testing-library/user-event";
import UserForm from "./UserForm";

test("it shows two inputs and a button", () => {
	// render the component

	render(<UserForm />);

	// Manipulate the component or find an element in it
	const inputs = screen.getAllByRole("textbox");

	const button = screen.getByRole("button");

	// Assertion - make sure the component is doing
	// what we expect it to do
	expect(inputs).toHaveLength(2);
	expect(button).toBeInTheDocument();
});

test("it calls onUserAdd when the form is submitted", async () => {
	const mock = jest.fn();

	// Try to render my component
	render(<UserForm onUserAdd={mock} />); // Find the two inputs

	const nameInput = screen.getByRole("textbox", {
		name: /name/i,
	});

	const emailInput = screen.getByRole("textbox", {
		name: /email/i,
	});

	// Simulate typing in a name
	await user.click(nameInput);

	await user.keyboard("jane");

	// Simulate typing in an email
	await user.click(emailInput);

	await user.keyboard("jane@jane.com");

	// Find the button
	const button = screen.getByRole("button");

	// Simulate clicking the button
	await user.click(button);

	// Assertion to make sure 'onUserAdd' gets called with email/name
	expect(mock).toHaveBeenCalled();
	expect(mock).toHaveBeenCalledWith({ name: "jane", email: "jane@jane.com" });
});

test("empties the two inputs when form is submitted", async () => {
	render(<UserForm onUserAdd={() => {}} />);

	const nameInput = screen.getByRole("textbox", { name: /name/i });
	const emailInput = screen.getByRole("textbox", { name: /email/i });
	const button = screen.getByRole("button");

	await user.click(nameInput);
	await user.keyboard("jane");
	await user.click(emailInput);
	await user.keyboard("jane@jane.com");

	await user.click(button);

	expect(nameInput).toHaveValue("");
	expect(emailInput).toHaveValue("");
});
```

---
