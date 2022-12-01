1. 
```ts
 const checkIfStringHasSpecialChar = (v: string) => {
  const spChars = /[!@#$%^&*()_+\-=\[\]{};':"\\|,.<>\/?]+/;
  return spChars.test(v);
};
```