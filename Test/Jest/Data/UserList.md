---
tags:
---

# UserList

## tool to use

Fortunately, there's a really nice tool.

We can use a nice little trick to help us understand how to find particular elements.

![[Pasted image 20240313080050.png]]

So this is not an error message that is popping up.
 
It is a little bit more of a warning.

```js
import { render } from "@testing-library/react";
screen.logTestingPlaygroundURL();
```

I'll then go back over to my terminal.

And where my tests are running, I'll now see that I get a console.log of a pretty long URL.

The URL is going to go to testing playground dot com.

You'll notice that there is a long kind of sequence of characters that is all of the HTML that your

component produced in an encoded format.

So this link right here includes all the HTML produced by our component.

Tick to select difficult elements
I want to get a suggested selector, but I can't click on the TR.

So as a workaround, I can go to the TR add in a style attribute with maybe a border ten picks solid

red.

And a display of lock.

So now that TW element has a huge red border around it and I can very easily select the row.

And so I can select it, click it, and I'll be given a suggested query right here.

Hey, if I do a get by row, row with row, I'll be given that element.

So you sometimes are going to have to use this little styling workaround again just to make sure elements

are actually clickable.
![[Pasted image 20240313082022.png]]


Another way is 
And when I run that test file, if I scroll up now here's the result of the screenshot debug.

It's going to take all the current rendered output from our component and print it out on the screen

so we can use this to very easily see what the current state or what the visible state really is of

our component.
```js
screen.debug()
```

## code

![[Pasted image 20240313082816.png]]
![[Pasted image 20240313082921.png]]
So there are two workarounds here to kind of escape hatches that we can use to find elements when this

role approach doesn't work the way we expect.

And we can take a look at both these escape hatches in this video and the next one.

So first fallback, number one.

- Fallback number one is to find elements by using an attribute called a data test ID.

Understanding what this data tested thing is all about is really easy.

If we just look at the code rather than diagrams or anything like that.

So let me just show you how we would find the number of rows by using data test ID.

> not very good

```jsx
<tbody data-testid="users">{renderedUsers}</tbody>
```

Well, first, inside of our components, we assigned a very special prop, a prop of data test ID,

and I assigned to that an arbitrary string.

A string that doesn't really have any meaning.

This could be users.
By assigning this prop right here, we are given the ability to select or find this particular element.

So we can now, inside of our test, write out some code that's going to find this t body element.

```js
import { render, screen, within } from "@testing-library/react";

import UserList from "./UserList";

function renderComponent() {
	const users = [
		{ name: "jane", email: "jane@jane.com" },
		{ name: "sam", email: "sam@sam.com" },
	];

	render(<UserList users={users} />);

	return {
		users,
	};
}

test("render one row per user", () => {
	// Render the component

	renderComponent(); // Find all the rows in the table

	const rows = within(screen.getByTestId("users")).getAllByRole("row");
	// Assertion: correct number of rows in the table

	expect(rows).toHaveLength(2);
});

test("render the email and name of each user", () => {
	const { users } = renderComponent();

	for (let user of users) {
		const name = screen.getByRole("cell", { name: user.name });

		const email = screen.getByRole("cell", { name: user.email });

		expect(name).toBeInTheDocument();

		expect(email).toBeInTheDocument();
	}
});
```

- Let's take a look at one other possible way of solving this problem.

So this will be used in the case where maybe assigning a test ID is not very easy, or maybe you just

don't like the idea of adding in this test ID thing to your components solely for the purposes of testing.

just use query selector

```js
// Find all the rows in the table
// eslint-disable-next-line
const rows = container.querySelectorAll("tbody tr");
```

---
