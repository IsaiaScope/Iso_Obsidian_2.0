# Decorator

> Facilitates the addition of behaviors to individual objects without inheriting from them.

The decorator pattern allows extending objects' behavior dynamically in runtime.

## Motivation

- Want to augment an object with additional functionality
- Do not want to rewrite or alter existing code (OCP)
- Want to keep new functionality separate (SRP)
- Need to be able to interact with existing structures
- Two options:
  - Inherit from required object (if possible)
  - Build a Decorator, which simply references the decorated object(s)

---

## Summary

- A decorator keeps the reference to the decorated object(s)
- Adds utility fields and methods to augment the object's features
- May or may not forward calls to the underlying object

---

## Example

Decorator is just something to add an additional functionality to a class, as fallow with add more functionality to a _Circle_ class

```js
class Shape {}

class Circle extends Shape {
	constructor(radius = 0) {
		super();

		this.radius = radius;
	}

	resize(factor) {
		this.radius *= factor;
	}

	toString() {
		return `A circle of radius ${this.radius}`;
	}
}

class Square extends Shape {
	constructor(side = 0) {
		super();

		this.side = side;
	}

	toString() {
		return `A square with side ${this.side}`;
	}
}

// we don't want ColoredSquare, ColoredCircle, etc.

class ColoredShape extends Shape {
	constructor(shape, color) {
		super();

		this.shape = shape;

		this.color = color;
	}

	toString() {
		return `${this.shape.toString()} ` + `has the color ${this.color}`;
	}
}

class TransparentShape extends Shape {
	constructor(shape, transparency) {
		super();

		this.shape = shape;

		this.transparency = transparency;
	}

	toString() {
		return (
			`${this.shape.toString()} has ` +
			`${this.transparency * 100.0}% transparency`
		);
	}
}

let circle = new Circle(2);

console.log(circle.toString());

let redCircle = new ColoredShape(circle, "red");

console.log(redCircle.toString());

// impossible: redHalfCircle is not a Circle

// redHalfCircle.resize(2);

let redHalfCircle = new TransparentShape(redCircle, 0.5);

console.log(redHalfCircle.toString());
```

---
