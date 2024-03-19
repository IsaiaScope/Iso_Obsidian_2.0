---
tags:
---

# hard concepts

![[Pasted image 20240319093845.png]]
![[Pasted image 20240319103047.png]]

act

![[Pasted image 20240319110208.png]]

```tsx
import { Link } from 'react-router-dom';
import { MarkGithubIcon } from '@primer/octicons-react';
import FileIcon from '../tree/FileIcon';
import RepositoriesSummary from './RepositoriesSummary';


function RepositoriesListItem({ repository }) {
  const { full_name, language, description, owner, name } = repository;
  return (
    <div className="py-3 border-b flex">
      <FileIcon name={language} className="shrink w-6 pt-1" />
      <div>
        <Link to={`/repositories/${full_name}`} className="text-xl">
          {owner.login}/<span className="font-bold">{name}</span>
        </Link>
        <p className="text-gray-500 italic py-1">{description}</p>
        <RepositoriesSummary repository={repository} />
      </div>
      <div className="grow flex items-center justify-end pr-2">
        <a href={repository.html_url} aria-label="github repository">
          <MarkGithubIcon />
        </a>
      </div>
    </div>
  );
}

export default RepositoriesListItem;
```

![[Pasted image 20240319110342.png]]
![[Pasted image 20240319110532.png]]

```jsx
import { render, screen } from "@testing-library/react";
import { MemoryRouter } from "react-router-dom";
import RepositoriesListItem from "./RepositoriesListItem";

function renderComponent() {
	const repository = {
		full_name: "facebook/react",
		language: "Javascript",
		description: "A js library",
		owner: {
			login: "facebook",
		},
		name: "react",
		html_url: "https://github.com/facebook/react",
	};

	render(
		<MemoryRouter>
			<RepositoriesListItem repository={repository} />
		</MemoryRouter>
	);

	return { repository };
}

test("shows a link to the github homepage for this repository", async () => {
	const { repository } = renderComponent();

	await screen.findByRole("img", { name: "Javascript" });

	const link = screen.getByRole("link", {
		name: /github repository/i,
	});

	expect(link).toHaveAttribute("href", repository.html_url);
});

test("shows a fileicon with the appropriate icon", async () => {
	renderComponent();

	const icon = await screen.findByRole("img", { name: "Javascript" });

	expect(icon).toHaveClass("js-icon");
});

test("shows a link to the code editor page", async () => {
	const { repository } = renderComponent();

	await screen.findByRole("img", { name: "Javascript" });

	const link = await screen.findByRole("link", {
		name: new RegExp(repository.owner.login),
	});

	expect(link).toHaveAttribute("href", `/repositories/${repository.full_name}`);
});
```


![[Pasted image 20240319110748.png]]

The other thing I want to mention right away is that you're going to see these act warnings very, very

frequently if you're doing data fetching inside of a use effect function.

So if you are expecting to work on an application and do some testing around anything that's going to

involve data fetching instead of using fact,

![[Pasted image 20240319110827.png]]

![[Pasted image 20240319184952.png]]

![[Pasted image 20240319185201.png]]

Okay, So let's imagine that we are trying to test a very simple component.

The goal of this component is to show a button, and whenever a user clicks on that button, we want

to make a network request to go and fetch some data, maybe get a list of users, and then show those

users on the screen.



---
