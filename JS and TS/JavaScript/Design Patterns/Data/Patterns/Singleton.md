# Singleton

> A component which is instantiated only once.

- For some components it only makes sense to have one in the system
  - Database repository
  - Object factory
- E.g., the constructor call is expensive
  - We want initialization to only happen once
  - We provide everyone with the same instance
- Want to prevent anyone creating additional copies

---

## Summary

## Example

```js
class Singleton {
	constructor() {
		const instance = this.constructor.instance;
		if (instance) {
			return instance;
		}

		this.constructor.instance = this;
	}

	foo() {
		console.log("Doing something...");
	}
}

let s1 = new Singleton();
let s2 = new Singleton();
console.log("Are they identical? " + (s1 === s2));
s1.foo();
```

---

## Example (Monostate)

```js
class Singleton {
	constructor() {
		const instance = this.constructor.instance;
		if (instance) {
			return instance;
		}

		this.constructor.instance = this;
	}

	foo() {
		console.log("Doing something...");
	}
}

let s1 = new Singleton();
let s2 = new Singleton();
console.log("Are they identical? " + (s1 === s2));
s1.foo();
```

---