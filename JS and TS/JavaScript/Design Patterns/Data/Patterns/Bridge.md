# Bridge

> A mechanism that decouples an interface (hierarchy) from an implementation (hierarchy). Reminder:
> JS has duck typing, so definitions of interfaces are not strictly necessary.

_Bridge pattern allows having one interface in our class build different implementations depending on what instance we are receiving and what instance we need to return._

## Motivation

- Bridge prevents a 'Cartesian product' complexity explosion
- Example:
  - Base class ThreadScheduler
  - Can be preemptive or cooperative
  - Can run on Windows or Unix
  - End up with with a 2×2 scenario: WindowsPTS, UnixPTS, WindowsCTS, UnixCTS
- Bridge pattern avoids the entity explosion

---

## Summary

- Decouple abstraction from implementation
- Both can exist as hierarchies
- A stronger form of encapsulation

---

## Example

In this example, we create a bridge between types of Shape and types of Render, in this way we can pass the instance of Render correctly to our Shapes.

```js
class VectorRenderer {
	renderCircle(radius) {
		console.log(`Drawing a circle of radius ${radius}`);
	}
}

class RasterRenderer {
	renderCircle(radius) {
		console.log(`Drawing pixels for circle of radius ${radius}`);
	}
}

class Shape {
	constructor(renderer) {
		this.renderer = renderer;
	}
}

class Circle extends Shape {
	constructor(renderer, radius) {
		super(renderer);
		this.radius = radius;
	}

	draw() {
		this.renderer.renderCircle(this.radius);
	}

	resize(factor) {
		this.radius *= factor;
	}
}

// imagine Square, Triangle
// different ways of rendering: vector, raster
// we don't want a cartesian product of these

let raster = new RasterRenderer();
let vector = new VectorRenderer();
let circle = new Circle(vector, 5);
circle.draw();
circle.resize(2);
circle.draw();
```

---
