```js
export const camelCaseToTitleCase = (text: string): string => {
  const result = text.replace(/([A-Z])/g, ' $1');
  const firstUp = result.charAt(0).toUpperCase();
  return `${firstUp}${result.slice(1)}`;
};
```

