1. ex
	- useful to create obj props dynamically
```js
function func1(a, b) {
  this[b] = a;
}

const a = {ciao: 'pino', ...Reflect.construct(func1, ['db', 'pinuccio'])}

// a => {ciao: 'pino', pinuccio: 'db'}
```