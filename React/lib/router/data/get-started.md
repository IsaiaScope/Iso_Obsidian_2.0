1. [lib](https://reactrouter.com/en/main)
2. [GitHubs source](https://github.com/academind/react-complete-guide-code/tree/20-building-mpas-with-react-router-updated/code/32-finished/frontend/src)
3. features
	1. end => useful for "/" path in navLink for set active just when "/" is matched and not always
	2. index => set default
	3. loader => function executed before routing, data isn't accessible bottom up, just deeping throught child route or same level
	4. errorElement => is triggered when some route code throw an error bottom up, if children throw an error on loader function errorElement is triggered
---
```jsx
```