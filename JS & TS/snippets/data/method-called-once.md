```ts
  once = (fn) => ((ran = false) => () => ran ? fn : ((ran = !ran), (fn = fn())))();
```