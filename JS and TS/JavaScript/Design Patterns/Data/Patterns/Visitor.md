# Visitor

> A component (visitor) that knows how to traverse a data structure composed of (possibly related) types.

The visitor pattern allows separate algorithms from the objects on which they operate.

## Motivation

- Need to define a new operation on an entire class hierarchy
  - E.g., make a document model printable to HTML/Markdown
- Do not want to keep modifying every class in the hierarchy
- Need access to the non-common aspects of classes in the hierarchy
- Create an external component to handle rendering
  - But avoid explicit type checks

---

## Summary

- Propagate an accept(Visitor v) method throughout the entire hierarchy
- Create a visitor with visitFoo (Foo), visitBar (Bar), ... for each element in the hierarchy
- Each accept() simply calls visitor.visitXxx(this)

---

## Example (Intrusive)

```js
class NumberExpression {
	constructor(value) {
		this.value = value;
	}

	print(buffer) {
		buffer.push(this.value.toString());
	}
}

class AdditionExpression {
	constructor(left, right) {
		this.left = left;

		this.right = right;
	}

	print(buffer) {
		buffer.push("(");

		this.left.print(buffer);

		buffer.push("+");

		this.right.print(buffer);

		buffer.push(")");
	}
}

// 1 + (2+3)

let e = new AdditionExpression(
	new NumberExpression(1),

	new AdditionExpression(new NumberExpression(2), new NumberExpression(3))
);

let buffer = [];

e.print(buffer);

console.log(buffer.join(""));
```

---

## Example (Reflective)

```js
class NumberExpression {
	constructor(value) {
		this.value = value;
	}
}

class AdditionExpression {
	constructor(left, right) {
		this.left = left;

		this.right = right;
	}
}

class ExpressionPrinter {
	print(e, buffer) {
		if (e instanceof NumberExpression) {
			buffer.push(e.value);
		} else if (e instanceof AdditionExpression) {
			buffer.push("(");

			this.print(e.left, buffer);

			buffer.push("+");

			this.print(e.right, buffer);

			buffer.push(")");
		}
	}
}

let e = new AdditionExpression(
	new NumberExpression(1),

	new AdditionExpression(new NumberExpression(2), new NumberExpression(3))
);

let buffer = [];

let ep = new ExpressionPrinter();

ep.print(e, buffer);

console.log(buffer.join(""));
```

---

## Example (Classic)

```js
class NumberExpression {
	constructor(value) {
		this.value = value;
	}

	accept(visitor) {
		visitor.visitNumber(this);
	}
}

class AdditionExpression {
	constructor(left, right) {
		this.left = left;

		this.right = right;
	}

	accept(visitor) {
		visitor.visitAddition(this);
	}
}

class Visitor {
	constructor() {
		this.buffer = [];
	}
}

class ExpressionPrinter extends Visitor {
	constructor() {
		super();
	}

	visitNumber(e) {
		this.buffer.push(e.value);
	}

	visitAddition(e) {
		this.buffer.push("(");

		e.left.accept(this);

		this.buffer.push("+");

		e.right.accept(this);

		this.buffer.push(")");
	}

	toString() {
		return this.buffer.join("");
	}
}

class ExpressionCalculator {
	// this visitor is stateful which can lead to problems

	constructor() {
		this.result = 0;
	}

	visitNumber(e) {
		this.result = e.value;
	}

	visitAddition(e) {
		e.left.accept(this);

		let temp = this.result;

		e.right.accept(this);

		this.result += temp;
	}
}

let e = new AdditionExpression(
	new NumberExpression(1),

	new AdditionExpression(new NumberExpression(2), new NumberExpression(3))
);

var ep = new ExpressionPrinter();

ep.visitAddition(e);

var ec = new ExpressionCalculator();

ec.visitAddition(e);

console.log(`${ep.toString()} = ${ec.result}`);
```

---
