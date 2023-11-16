# Open-Closed Principle

_OCP_ means open for extension, closed for modification

The general idea of this principle is great. It tells you to write your code so that you will be able to add new functionality without changing the existing code.

Robert C. Martin in his articles about the SOLID principles said, inheritance introduces tight coupling if the subclasses depend on implementation details of their parent class.

so he uses _interfaces instead of superclasses_ to allow different implementations which you can easily substitute without changing the code that uses them. The interfaces are closed for modifications, and you can provide new implementations to extend the functionality of your software.

The main benefit of this approach is that an interface introduces an additional level of abstraction which enables loose coupling. The implementations of an interface are independent of each other and don’t need to share any code. If you consider it beneficial that two implementations of an interface share some code, you can either use [inheritance](https://stackify.com/oop-concept-inheritance/) or [composition](https://stackify.com/oop-concepts-composition/).

## Example

Filter by a criteria, the way to approach is with specification pattern because is difficult to handle all cases [1]. In this way of thinking specification meas _inheritability class_ (N.B. not super classes) to separate a criteria and verify the criteria just one time [2]

```js
let Color = Object.freeze({
	red: "red",
	green: "green",
	blue: "blue",
});

let Size = Object.freeze({
	small: "small",
	medium: "medium",
	large: "large",
	yuge: "yuge",
});

class Product {
	constructor(name, color, size) {
		this.name = name;
		this.color = color;
		this.size = size;
	}
}

class ProductFilter {
	filterByColor(products, color) {
		return products.filter((p) => p.color === color);
	}

	filterBySize(products, size) {
		return products.filter((p) => p.size === size);
	}

	filterBySizeAndColor(products, size, color) {
		return products.filter((p) => p.size === size && p.color === color);
	}
	// state space explosion
	// 3 criteria (+ weight) = 7 methods to handle all cases we need a better approach
	// and soon if we're goona add more filter criteria

	// OCP = open for extension, closed for modification
}

let apple = new Product("Apple", Color.green, Size.small);
let tree = new Product("Tree", Color.green, Size.large);
let house = new Product("House", Color.blue, Size.large);
let products = [apple, tree, house];

// let pf = new ProductFilter();
// console.log(`Green products (old):`);
// for (let p of pf.filterByColor(products, Color.green))
//   console.log(` * ${p.name} is green`);
// ↑↑↑ BEFORE [1]

// ↓↓↓ AFTER [2]
// general interface for a specification
class ColorSpecification {
	constructor(color) {
		this.color = color;
	}
	isSatisfied(item) {
		return item.color === this.color;
	}
}

class SizeSpecification {
	constructor(size) {
		this.size = size;
	}
	isSatisfied(item) {
		return item.size === this.size;
	}
}

class BetterFilter {
	filter(items, spec) {
		return items.filter((x) => spec.isSatisfied(x));
	}
}

// specification combinator
class AndSpecification {
	constructor(...specs) {
		this.specs = specs;
	}
	isSatisfied(item) {
		return this.specs.every((x) => x.isSatisfied(item));
	}
}

let bf = new BetterFilter();
console.log(`Green products (new):`);

for (let p of bf.filter(products, new ColorSpecification(Color.green))) {
	console.log(` * ${p.name} is green`);
}

console.log(`Large products:`);

for (let p of bf.filter(products, new SizeSpecification(Size.large))) {
	console.log(` * ${p.name} is large`);
}

console.log(`Large and green products:`);

let spec = new AndSpecification(
	new ColorSpecification(Color.green),
	new SizeSpecification(Size.large)
);

for (let p of bf.filter(products, spec))
	console.log(` * ${p.name} is large and green`);
```

---
