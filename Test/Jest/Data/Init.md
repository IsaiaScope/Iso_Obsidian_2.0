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

Right now, whenever we render user form, we are not passing in any props to it whatsoever.

No props.

So inside of user form, whenever we render this thing, it is not being given an on user ad prop.

We did not provide one.

So when we simulate submitting the form handle submit is called, but there is no function called on

user ad we did not provide it.

So on user ad is essentially undefined and that's why we are seeing an error message whenever we run

our tests right now.

So we need to fix this up.

One way we could fix this up is by going back over to our test and putting in right here on user ad.

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

mock

A mock function is a fake function that doesn't really do anything when it is called.

All it does is record the fact that it got called and also records the arguments that it was called

with.

We most often use mock functions whenever we need to make sure that a component actually calls a callback.

The mock function is going to have some internal storage of sorts.

It's going to record how many times it has been called.

It's also going to record all the different arguments it receives whenever it gets called.
![[Pasted image 20240311130950.png]]

NOTE
So this is a label and an input element.

I want to tell you a little bit around some normal traditional HTML stuff.

So this is not really a React specific.

If you are ever creating a label with an input next to it, you can give the label a for attributes.

In JSX, we have to write it out as HTML four, but if you are doing normal HTML, it would be just

for if a label has an HTML four that is equal to an input elements ID attribute.

Then clicking on the label is going to focus the input.
![[Pasted image 20240313075036.png]]

So this is just a little accessibility and usability thing, particularly for mobile devices where a

user might accidentally tap on the label when they really mean to select the input.

BIND INPUTS
It's going to find some label that has text that matches this regular expression of enter email and

it's going to give us the input element that is associated with that label.

So either of these query functions would be absolutely appropriate to use.

Now React Testing Library itself prefers or recommends that you use rolls to select elements.

```js
// Find the two inputs

const nameInput = screen.getByRole("textbox", {
	name: /name/i,
});

const emailInput = screen.getByRole("textbox", {
	name: /email/i,
});
```

---


