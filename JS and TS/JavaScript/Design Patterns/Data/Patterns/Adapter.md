# Adapter

> A construct which adapts an existing interface X to conform to the required interface Y.

## Examples for Understanding

- Electrical devices have different power (interface) requirements
  - Voltage (5V, 220V)
  - Socket/plug type (Europe, UK, USA)
- We cannot modify our gadgets to support every possible interface
  - Some support possible (e.g., 120/220V)
- Thus, we use a special device (an adapter) to give us the interface we require from the interface we have

---

_Adapter_ is a special object that converts the interface of one object so that another object can understand it.

An adapter wraps one of the objects to hide the complexity of conversion happening behind the scenes. The wrapped object isn’t even aware of the adapter. For example, you can wrap an object that operates in meters and kilometers with an adapter that converts all of the data to imperial units such as feet and miles.

Adapters can not only convert data into various formats but can also help objects with different interfaces collaborate. Here’s how it works:

- The adapter gets an interface, compatible with one of the existing objects.
- Using this interface, the existing object can safely call the adapter’s methods.
- Upon receiving a call, the adapter passes the request to the second object, but in a format and order that the second object expects.

Sometimes it’s even possible to create a two-way adapter that can convert the calls in both directions.

---

## Summary

- Implementing an Adapter is easy
- Determine the API you have and the API you need
- Create a component which aggregates (has a reference to, ...) the adaptee
- Intermediate representations can pile up: use caching and other optimizations

---

## Example

Transform a line in a set of points (`LineToPointAdapter`), _there is a problem using an adapter we generating a temporary object so we can avoid with cashing and the solution is to generate the data once and reuse that the fallowing times we need it_

```js
class Point {
	constructor(x, y) {
		this.x = x;
		this.y = y;
	}

	toString() {
		return `(${this.x}, ${this.y})`;
	}
}

class Line {
	constructor(start, end) {
		this.start = start;
		this.end = end;
	}

	toString() {
		return `${this.start.toString()}→${this.end.toString()}`;
	}
}

class VectorObject extends Array {}

class VectorRectangle extends VectorObject {
	constructor(x, y, width, height) {
		super();
		this.push(new Line(new Point(x, y), new Point(x + width, y)));
		this.push(
			new Line(new Point(x + width, y), new Point(x + width, y + height))
		);
		this.push(new Line(new Point(x, y), new Point(x, y + height)));
		this.push(
			new Line(new Point(x, y + height), new Point(x + width, y + height))
		);
		this.push;
	}
}
// ↑↑↑ this is your API ↑↑↑

// ↓↓↓ this is what you have to work with ↓↓↓
let vectorObjects = [
	new VectorRectangle(1, 1, 10, 10),
	new VectorRectangle(3, 3, 6, 6),
];

let drawPoint = function (point) {
	process.stdout.write(".");
};

// ↓↓↓ to draw our vector objects, we need an adapter ↓↓↓
class LineToPointAdapter extends Array {
	constructor(line) {
		super();
		console.log(
			`${LineToPointAdapter.count++}: Generating ` +
				`points for line ${line.toString()} (no caching)`
		);

		let left = Math.min(line.start.x, line.end.x);
		let right = Math.max(line.start.x, line.end.x);
		let top = Math.min(line.start.y, line.end.y);
		let bottom = Math.max(line.start.y, line.end.y);

		if (right - left === 0) {
			for (let y = top; y <= bottom; ++y) {
				this.push(new Point(left, y));
			}
		} else if (line.end.y - line.start.y === 0) {
			for (let x = left; x <= right; ++x) {
				this.push(new Point(x, top));
			}
		}
	}
}
LineToPointAdapter.count = 0;

let drawPoints = function () {
	for (let vo of vectorObjects)
		for (let line of vo) {
			let adapter = new LineToPointAdapter(line);
			adapter.forEach(drawPoint);
		}
};

drawPoints();
drawPoints();
```

---

## Example (Adapter Caching)

_Caching_ is important to avoid that adapter create a new data every time because that is temporary we need to cache data the first time and then reuse it.
Make some modification to previous example we obtain: using an hash code [1] we store the points and we dont store anymore is hash is already inside the cache instead we reuse the existing data

```js
class Point {
	constructor(x, y) {
		this.x = x;
		this.y = y;
	}

	toString() {
		return `(${this.x}, ${this.y})`;
	}
}

class Line {
	constructor(start, end) {
		this.start = start;
		this.end = end;
	}

	toString() {
		return `${this.start.toString()}→${this.end.toString()}`;
	}
}

class VectorObject extends Array {}

class VectorRectangle extends VectorObject {
	constructor(x, y, width, height) {
		super();
		this.push(new Line(new Point(x, y), new Point(x + width, y)));
		this.push(
			new Line(new Point(x + width, y), new Point(x + width, y + height))
		);
		this.push(new Line(new Point(x, y), new Point(x, y + height)));
		this.push(
			new Line(new Point(x, y + height), new Point(x + width, y + height))
		);
		this.push;
	}
}

// ↑↑↑ this is your API ↑↑↑

// ↓↓↓ this is what you have to work with ↓↓↓
String.prototype.hashCode = function () {
	// [1]
	if (Array.prototype.reduce) {
		return this.split("").reduce(function (a, b) {
			a = (a << 5) - a + b.charCodeAt(0);
			return a & a;
		}, 0);
	}
	let hash = 0;
	if (this.length === 0) return hash;
	for (let i = 0; i < this.length; i++) {
		const character = this.charCodeAt(i);
		hash = (hash << 5) - hash + character;
		hash = hash & hash; // Convert to 32-bit integer
	}
	return hash;
};

class LineToPointAdapter extends Array {
	constructor(line) {
		super();

		this.hash = JSON.stringify(line).hashCode();
		if (LineToPointAdapter.cache[this.hash]) return; // we already have it

		console.log(
			`${LineToPointAdapter.count++}: Generating ` +
				`points for line ${line.toString()} (with caching)`
		);

		let points = [];

		let left = Math.min(line.start.x, line.end.x);
		let right = Math.max(line.start.x, line.end.x);
		let top = Math.min(line.start.y, line.end.y);
		let bottom = Math.max(line.start.y, line.end.y);

		if (right - left === 0) {
			for (let y = top; y <= bottom; ++y) {
				points.push(new Point(left, y));
			}
		} else if (line.end.y - line.start.y === 0) {
			for (let x = left; x <= right; ++x) {
				points.push(new Point(x, top));
			}
		}

		LineToPointAdapter.cache[this.hash] = points;
	}

	get items() {
		return LineToPointAdapter.cache[this.hash];
	}
}
LineToPointAdapter.count = 0;
LineToPointAdapter.cache = {};

let vectorObjects = [
	new VectorRectangle(1, 1, 10, 10),
	new VectorRectangle(3, 3, 6, 6),
];

let drawPoint = function (point) {
	process.stdout.write(".");
};

let draw = function () {
	for (let vo of vectorObjects) {
		for (let line of vo) {
			let adapter = new LineToPointAdapter(line);
			adapter.items.forEach(drawPoint);
		}
	}
};

draw();
draw();
```

---
