- The *test()* method executes a search for a match between a regular expression and a specified string. Returns  true or  false.
````ad-example
title: *checkSpecialChar*
```ts
const checkSpecialChar = (v: string): boolean => {
	const spChars = /[!@#$%^&*()_+\-=\[\]{};':"\\|,.<>\/?]+/;
	return spChars.test(v);
};
```
````

