---
tags:

- Tailwind
---

# [Tailwind CSS](https://tailwindcss.com/) (clsx + twMerge)

```
npm i --save tailwind tailwind-merge clsx
```

## [twMerge](https://www.npmjs.com/package/tailwind-merge)

`twMerge` provides an utility function to efficiently merge classes in JS without style conflicts because when you have a conflict in tailwind isn't follow order of specification; it's pretty much random and not predictable.

> Simple example where `Home` component pass to `Button` component a new value for `bg` and `twMerge` handle the correct merging, is possible also adding conditional style with `useState` and `twMerge` handle the correct order and flow of style application

```jsx
import Button from "@/components/button";
// Home component
export default function Home() {
	return (
		<main className="flex justify-center p-24">
			<Button className="bg-green-500" />
		</main>
	);
}
```

```jsx
// Button component
export default function Button({ className }) {
	const [pending, setPending] = useState(false);

	return (
		<button
			className={twMerge(
				"bg-blue-500 py-2 px-4",
				className,
				pending && "bg-gray-500"
			)}
		>
			Submit
		</button>
	);
}
```

### Problem with twMerge

> With `twMerge` isn't possible to use objects to render conditionally the style; so there is another library: `clsx` that merged together make possible to add objects to `twMerge`

```jsx
// this code doesn't work :(
export default function Button({ className }) {
	const [pending, setPending] = useState(false);

	return (
		<button
			className={twMerge("•bg-blue-500 py-2 px-4", className, {
				"@bg-gray-500": pending,
			})}
		>
			Submit    
		</button>
	);
}
```

## [clsx](https://www.npmjs.com/package/clsx)

adding `clsx` the code becomes as follow but without `twMerge` merging is granted no more.

---

## Solution (Utility class)

Using `twMerge` and `clsx` in an utility class is possible to merge style in the correct way and use objects inside `twMerge`

> create a utilityFile.ts

```ts
import { type ClassValue, clsx } from "clsx"
import { twMerge } from "tailwind-merge"

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}
```

> use utility functions inside components to style

```jsx
// this code works but style conflicts is random and order is not granted :(
export default function Button({ className, ...props }) {
	const [pending, setPending] = useState(false);

	return (
		<button
			className={cn("•bg-blue-500 py-2 px-4", className, {
				"@bg-gray-500": pending,
			})}
			{...props}
		>
			Submit    
		</button>
	);
}
```

---
