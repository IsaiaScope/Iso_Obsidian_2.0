# Composite

> A mechanism for treating individual (scalar) objects and compositions of objects in a uniform manner.

- Objects use other objects' fields/methods through inheritance and composition
- Composition lets us make compound objects
  - E.g., a mathematical expression composed of simple expressions; or
  - A shape group made of several different shapes
- Composite design pattern is used to treat both single (scalar) and composite objects uniformly
  - I.e., class Foo and an array (containing Foos) having the same API

The composite pattern allows the creation of objects with properties that are primitive items or a collection of objects. Each item in the collection can hold other collections themselves, creating deeply nested structures.

## Summary

---

## Example

Group `GraphicObject` together in a container `children` treating all objects/data in the same and uniform manner

```js
class GraphicObject {
	get name() {
		return this._name;
	}

	constructor(name = "Group " + GraphicObject.count++) {
		this.children = [];

		this.color = undefined;

		this._name = name;
	}

	print(buffer, depth) {
		buffer.push("*".repeat(depth));

		if (depth > 0) buffer.push(" ");

		if (this.color) buffer.push(this.color + " ");

		buffer.push(this.name);

		buffer.push("\n");

		for (let child of this.children) child.print(buffer, depth + 1);
	}

	toString() {
		let buffer = [];

		this.print(buffer, 0);

		return buffer.join("");
	}
}

GraphicObject.count = 0;

class Circle extends GraphicObject {
	constructor(color) {
		super("Circle");

		this.color = color;
	}
}

class Square extends GraphicObject {
	constructor(color) {
		super("Square");

		this.color = color;
	}
}

let drawing = new GraphicObject();
drawing.children.push(new Square("Red"));
drawing.children.push(new Circle("Yellow"));

let group = new GraphicObject();
group.children.push(new Circle("Blue"));
group.children.push(new Square("Blue"));
drawing.children.push(group);

console.log(drawing.toString());
```

---
