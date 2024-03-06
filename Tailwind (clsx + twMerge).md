---
tags:

- Tailwind
---

# [Tailwind CSS](https://tailwindcss.com/) (clsx + twMerge)

```
npm i tailwind tailwind-merge clsx
```

## [twMerge](https://www.npmjs.com/package/tailwind-merge)

`twMerge` provides an utility function to efficiently merge classes in JS without style conflicts because when you have a conflict in tailwind isn't follow order of specification; it's pretty much random and not predictable.

```jsx
import Button from "@/components/button";
export default function Home() {
	return (
		<main className="flex justify-center p-24">
			<Button className="bg-green-500" />
		</main>
	);
}
```

---
