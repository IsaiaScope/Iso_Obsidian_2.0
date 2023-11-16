# Singleton

> A component which is instantiated only once.

## Motivation

- For some components it only makes sense to have one in the system
  - Database repository
  - Object factory
- E.g., the constructor call is expensive
  - We want initialization to only happen once
  - We provide everyone with the same instance
- Want to prevent anyone creating additional copies

---

## Summary

- A constructor can choose what to return; we can keep returning same instance
- Monostate: many instances, shared data
- Directly depending on the Singleton is a bad idea; introduce a dependency instead

---

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

it doesn't do any constructor magic, making every instance sharing the same data, a variation but the advice is to not use it

```js
class ChiefExecutiveOfficer {
	get name() {
		return ChiefExecutiveOfficer._name;
	}

	set name(value) {
		ChiefExecutiveOfficer._name = value;
	}

	get age() {
		return ChiefExecutiveOfficer._age;
	}

	set age(value) {
		ChiefExecutiveOfficer._age = value;
	}

	toString() {
		return `CEO's name is ${this.name} ` + `and he is ${this.age} years old.`;
	}
}

ChiefExecutiveOfficer._age = undefined;
ChiefExecutiveOfficer._name = undefined;

let ceo = new ChiefExecutiveOfficer();
ceo.name = "Adam Smith";
ceo.age = 55;

let ceo2 = new ChiefExecutiveOfficer();
ceo2.name = "John Gold";
ceo2.age = 66;

console.log(ceo.toString());
console.log(ceo2.toString());
```

---
