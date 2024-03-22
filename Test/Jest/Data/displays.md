```jsx
import { StarIcon } from "@primer/octicons-react";

function RepositoriesSummary({ repository }) {
	const { stargazers_count, open_issues, forks, language } = repository;

	return (
		<div className="flex flex-row gap-4 text-gray-700">
			<div>
				<StarIcon aria-label="stars" size={16} /> {stargazers_count}     
			</div>
			 <div>{open_issues} issues need help</div> 
			 <div>{forks} Forks	</div> 
			 <div>{language}</div> 
		</div>
	);
}

export default RepositoriesSummary;
```

```jsx
import { screen, render } from "@testing-library/react";

import RepositoriesSummary from "./RepositoriesSummary";

test("displays information about the repository", () => {
	const repository = {
		language: "Javascript",

		stargazers_count: 5,

		forks: 30,

		open_issues: 1,
	};

	render(<RepositoriesSummary repository={repository} />);

	for (let key in repository) {
		const value = repository[key];

		const element = screen.getByText(new RegExp(value));

		expect(element).toBeInTheDocument();
	}
});
```
