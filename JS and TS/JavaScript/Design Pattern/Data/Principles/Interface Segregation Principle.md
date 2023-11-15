# Interface Segregation Principle

> A client should never be forced to implement an interface that it doesn't use, or clients shouldn't be forced to depend on methods they do not use

- Divide big interfaces into smaller interfaces
- Smaller interfaces do separate tasks
- multiple inheritance in interface is possible

## Examples

`aggregation()` [1] is a method for inherit multiple classes `class Photocopier extends aggregation(Printer, Scanner)` and is not pertinent with the core functionality of the Interface Segregation Principle; is't just a plus

Bad approach [3] every "interface" repeat itself, _MultiFunctionPrinter_, _OldFashionedPrinter_ implements all _Machine_ methos

The Solution [4] makes _Photocopier_ to be sum of _Printer_ and _Scanner_

```js
var aggregation = (baseClass, ...mixins) => {
	// [1]
	class base extends baseClass {
		constructor(...args) {
			super(...args);
			mixins.forEach((mixin) => {
				copyProps(this, new mixin());
			});
		}
	}

	let copyProps = (target, source) => {
		// this function copies all properties and symbols, filtering out some special ones
		Object.getOwnPropertyNames(source)
			.concat(Object.getOwnPropertySymbols(source))
			.forEach((prop) => {
				if (
					!prop.match(
						/^(?:constructor|prototype|arguments|caller|name|bind|call|apply|toString|length)$/
					)
				)
					Object.defineProperty(
						target,
						prop,
						Object.getOwnPropertyDescriptor(source, prop)
					);
			});
	};

	mixins.forEach((mixin) => {
		// outside constructor() to allow aggregation(A,B,C).staticFunction() to be called etc.
		copyProps(base.prototype, mixin.prototype);
		copyProps(base, mixin);
	});
	return base;
};

class Document {}

// Bad approach [3]
class Machine {
	constructor() {
		if (this.constructor.name === "Machine")
			throw new Error("Machine is abstract!");
	}
	print(doc) {}
	fax(doc) {}
	scan(doc) {}
}

class MultiFunctionPrinter extends Machine {
	print(doc) {}
	fax(doc) {}
	scan(doc) {}
}

class NotImplementedError extends Error {
	constructor(name) {
		let msg = `${name} is not implemented!`;

		super(msg); // maintain proper stack trace

		if (Error.captureStackTrace)
			Error.captureStackTrace(this, NotImplementedError); // your custom stuff here :)
	}
}

class OldFashionedPrinter extends Machine {
	print(doc) {
		// ok
	} // omitting this is the same as no-op impl

	// fax(doc) {
	// do nothing
	// }

	scan(doc) {
		// throw new Error('not implemented!');
		throw new NotImplementedError("OldFashionedPrinter.scan");
	}
}

// The Solution [4]
class Printer {
	constructor() {
		if (this.constructor.name === "Printer")
			throw new Error("Printer is abstract!");
	}

	print() {}
}

class Scanner {
	constructor() {
		if (this.constructor.name === "Scanner")
			throw new Error("Scanner is abstract!");
	}

	scan() {}
}

class Photocopier extends aggregation(Printer, Scanner) {
	// [2]
	print() {
		// IDE won't help you here
	}
	scan() {}
}

// we don't allow this!
// let m = new Machine();

let printer = new OldFashionedPrinter();
printer.fax(); // nothing happens
//printer.scan();
```

---
