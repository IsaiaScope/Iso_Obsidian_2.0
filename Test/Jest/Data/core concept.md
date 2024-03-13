---
tags:
---

# core concept

The most important things that I want to walk away from this application with is just a basic understanding

of what we're doing when we are writing out a test.

So we call a test function.

Put in some description, we're going to render our components, go through some amount of setup, which

is almost always going to involve trying to find an element that was rendered by our components, to

find elements that were rendered by our components.

We're going to use those query functions.

They have names like get by role, get all by role and so on.

There are many different query functions available to us.

Memorizing them all is a little bit challenging, but over time we're going to see that they all follow

the same naming structure.

So you're going to eventually be able to use these different query functions without memorize them just

because you know what the word get implies and what the word role implies.

We also learned about how a different HTML elements sometimes have an implicit or automatically assigned

ARIA role.

Whenever we are using React Testing Library, we really like to select elements by using this role approach,

and the reason for that is that it just makes our tests a little bit more flexible.

We can go back and change or move around different elements without breaking the underlying test, which

is sometimes desirable.

Last thing we learned about was expectations.

So down here we put down an expectation also referred to as an assertion.

The entire line is an assertion or expectation.

And the second part here, we referred to that as the matcher.

There are many different matters available to us.

Some of them are in the default JAST testing library, others are implemented in testing library.

Jess Dom is the name of it.

```js
import { render, screen } from "@testing-library/react";

import user from "@testing-library/user-event";

import App from "./App";

test("can receive a new user and show it on a list", async () => {
	render(<App />);

	const nameInput = screen.getByRole("textbox", {
		name: /name/i,
	});

	const emailInput = screen.getByRole("textbox", {
		name: /email/i,
	});

	const button = screen.getByRole("button");

	await user.click(nameInput);

	await user.keyboard("jane");

	await user.click(emailInput);

	await user.keyboard("jane@jane.com");

	await user.click(button);

	const name = screen.getByRole("cell", { name: "jane" });

	const email = screen.getByRole("cell", { name: "jane@jane.com" });

	expect(name).toBeInTheDocument();

	expect(email).toBeInTheDocument();
});
```

---
