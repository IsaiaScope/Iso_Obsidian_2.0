# Observer

> An observer is an object that wishes to be informed about events happening in the system. The entity generating the events is an observable.

## Motivation

- We need to be informed when certain things happen
  - Object's property changes
  - Object does something
  - Some external event occurs
- We want to listen to events and be notified when they occur
  - Notifications should include useful data
- Want to unsubscribe from events if we're no longer interested

---

## Summary

---

## Example (Events)

```js
class Event {
	constructor() {
		this.handlers = new Map();

		this.count = 0;
	}

	subscribe(handler) {
		this.handlers.set(++this.count, handler);

		return this.count;
	}

	unsubscribe(idx) {
		this.handlers.delete(idx);
	}

	// 1) who fired the event?
	// 2) additional data (event args)

	fire(sender, args) {
		this.handlers.forEach((v, k) => v(sender, args));
	}
}

class FallsIllArgs {
	constructor(address) {
		this.address = address;
	}
}

class Person {
	constructor(address) {
		this.address = address;

		this.fallsIll = new Event();
	}

	catchCold() {
		this.fallsIll.fire(this, new FallsIllArgs(this.address));
	}
}

let person = new Person("123 London Road");

let sub = person.fallsIll.subscribe((s, a) => {
	console.log(`A doctor has been called ` + `to ${a.address}`);
});

person.catchCold();
person.catchCold();
person.fallsIll.unsubscribe(sub);
person.catchCold();
```

---
