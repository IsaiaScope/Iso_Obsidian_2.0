# Proxy

> A class that functions as an interface to a particular resource. That resource may be remote, expensive to construct, or may require logging or some other added functionality.

The Proxy pattern provides a surrogate or placeholder object for another object and controls access to this other object.

## Motivation

- You are calling foo.Bar()
- This assumes that foo is in the same process as Bar()
- What if, later on, you want to put all Foo-related operations into a separate process
  - Can you avoid changing your code?
- Proxy to the rescue!
  - Same interface, entirely different behavior
- This is called a communication proxy
  - Other types: logging, virtual, guarding, ecc...

---

## Summary

- A proxy has the same interface as the underlying object
- To create a proxy, simply replicate the existing interface of an object
- Add relevant functionality to the redefined member functions
- Different proxies (communication, logging, caching, etc.) have completely different behaviors

---

## Example (Value Proxy)

So eventually, once you've constructed 5% you can treat it as if it were a number and it keeps it two string implementation showing the actual percentage. This example is for numbers but works also with strings

```js
class Percentage {
	constructor(percent) {
		this.percent = percent; // 0 to 100
	}

	toString() {
		return `${this.percent}%`;
	}

	valueOf() {
		return this.percent / 100;
	}
}

let fivePercent = new Percentage(5);

console.log(`${fivePercent} of 50 is ${50 * fivePercent}`);
```

---

## Example (Property Proxy)

assign extra prop to a object like building a character

```js
class Property {
	constructor(value, name = "") {
		this._value = value;
		this.name = name;
	}

	get value() {
		return this._value;
	}
	set value(newValue) {
		if (this._value === newValue) return;
		console.log(`Assigning ${newValue} to ${this.name}`);
		this._value = newValue;
	}
}

class Creature {
	constructor() {
		this._agility = new Property(10, "agility");
	}

	get agility() {
		return this._agility.value;
	}
	set agility(value) {
		this._agility.value = value;
	}
}

let c = new Creature();
c.agility = 12;
c.agility = 13;
```

---

## Example (Protection Proxy)

control access to resources

```js
class Car {
	drive() {
		console.log("Car being driven");
	}
}

class CarProxy {
	constructor(driver) {
		this.driver = driver;

		this._car = new Car();
	}

	drive() {
		if (this.driver.age >= 16) this._car.drive();
		else console.log("Driver too young");
	}
}

class Driver {
	constructor(age) {
		this.age = age;
	}
}

let car = new Car();

car.drive();

let car2 = new CarProxy(new Driver(12)); // try 22

car2.drive();
```

---

## Example (Virtual Proxy)

express the concept off skeleton while you dont have the img show something else

```js
class Image {
	constructor(url) {
		this.url = url;

		console.log(`Loading image from ${this.url}`);
	}

	draw() {
		console.log(`Drawing image ${this.url}`);
	}
}

class LazyImage {
	constructor(url) {
		this.url = url;
	}

	draw() {
		if (!this.image) this.image = new Image(this.url);

		this.image.draw();
	}
}

function drawImage(img) {
	console.log("About to draw the image");

	img.draw();

	console.log("Done drawing the image");
}

let img = new LazyImage("http://pokemon.com/pikachu.png");

drawImage(img);
```

---
