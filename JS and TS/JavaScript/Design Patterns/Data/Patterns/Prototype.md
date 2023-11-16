# Prototype

> A partially or fully initialized object that you copy (clone) and make use of.

- Complicated objects (e.g., cars) aren't designed from scratch
  - They reiterate existing designs
- An existing (partially or fully constructed) design is a Prototype
- We make a copy (clone) the prototype and customize it
  - Requires 'deep copy' support
- We make the cloning convenient (e.g., via a Factory)

---

## Summary

## Example

_john_ is a prototype and you can make copy of that e customize for what you need, there is a limitation because you need a _deep copy_ for every property because the space in memory is the same so we need to separate and each copy need its space in memory

```js
class Address {
	constructor(streetAddress, city, country) {
		this.streetAddress = streetAddress;

		this.city = city;

		this.country = country;
	}

	deepCopy() {
		return new Address(this.streetAddress, this.city, this.country);
	}

	toString() {
		return `Address: ${this.streetAddress}, ` + `${this.city}, ${this.country}`;
	}
}

class Person {
	constructor(name, address) {
		this.name = name;

		this.address = address; //!
	}

	deepCopy() {
		return new Person(
			this.name,

			this.address.deepCopy() // needs to be recursive
		);
	}

	toString() {
		return `${this.name} lives at ${this.address}`;
	}
}

let john = new Person("John", new Address("123 London Road", "London", "UK"));

let jane = john.deepCopy();

jane.name = "Jane";

jane.address.streetAddress = "321 Angel St"; // oops

console.log(john.toString()); // oops, john is called 'jane'

console.log(jane.toString());
```

---

## Example

Idea behind _Copy Through Serialization_ is instead use _deep copy_ we can serialize (destructing) the object e rebuild that from scratch avoiding to create deep copy for each property.

```js
class Address {
	constructor(streetAddress, city, country) {
		this.streetAddress = streetAddress;
		this.city = city;
		this.country = country;
	}

	toString() {
		return `Address: ${this.streetAddress}, ` + `${this.city}, ${this.country}`;
	}
}

class Person {
	constructor(name, address) {
		this.name = name;
		this.address = address; //!
	}

	toString() {
		return `${this.name} lives at ${this.address}`;
	}

	greet() {
		console.log(
			`Hi, my name is ${this.name}, ` + `I live at ${this.address.toString()}`
		);
	}
}

class Serializer {
	constructor(types) {
		this.types = types;
	}

	markRecursive(object) {
		// anoint each object with a type index
		let idx = this.types.findIndex((t) => {
			return t.name === object.constructor.name;
		});
		if (idx !== -1) {
			object["typeIndex"] = idx;

			for (let key in object) {
				if (object.hasOwnProperty(key) && object[key] != null)
					this.markRecursive(object[key]);
			}
		}
	}

	reconstructRecursive(object) {
		if (object.hasOwnProperty("typeIndex")) {
			let type = this.types[object.typeIndex];
			let obj = new type();
			for (let key in object) {
				if (object.hasOwnProperty(key) && object[key] != null) {
					obj[key] = this.reconstructRecursive(object[key]);
				}
			}
			delete obj.typeIndex;
			return obj;
		}
		return object;
	}

	clone(object) {
		this.markRecursive(object);
		let copy = JSON.parse(JSON.stringify(object));
		return this.reconstructRecursive(copy);
	}
}

let john = new Person("John", new Address("123 London Road", "London", "UK"));

let jane = JSON.parse(JSON.stringify(john));

jane.name = "Jane";
jane.address.streetAddress = "321 Angel St";

john.greet();
// this won't work
// jane.greet();

// try a dedicated serializer
let s = new Serializer([Person, Address]); // pain point
jane = s.clone(john);

jane.name = "Jane";
jane.address.streetAddress = "321 Angel St";

console.log(john.toString());
console.log(jane.toString());
```

---

## Example

_Prototype Factory_

```js

```

---
