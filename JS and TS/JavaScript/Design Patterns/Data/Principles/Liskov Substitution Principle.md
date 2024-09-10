# Liskov Substitution Principle

> It’s the ability to replace any object of a parent class with any object of one of its child classes without affecting the correctness of the program.

I know it sounds strange to you but let’s break it into pieces. Suppose we have a program that has a parent class. The parent class has some child classes who inherit from. If we decided to create some objects from the parent class in our program, we’ve to be able to replace any one of them with any object of any child class, and the program should work as expected without any errors.

## Example

`useIt()` [1] breaks the functionality because brings _inheritability_ so is a bad approach of programming and against the ability to replace any object of a parent class with any object of one of its child, every child must depend on parent prop but don't manipulating those

`useIt()` if takes as input a Rectangle or a Square the output must be the same because Rectangle or a Square should be replaceable each other

```js
class Rectangle {
	constructor(width, height) {
		this._width = width;
		this._height = height;
	}

	get width() {
		return this._width;
	}

	get height() {
		return this._height;
	}

	set width(value) {
		this._width = value;
	}

	set height(value) {
		this._height = value;
	}

	get area() {
		return this._width * this._height;
	}

	toString() {
		return `${this._width}×${this._height}`;
	}
}

class Square extends Rectangle {
	constructor(size) {
		super(size, size);
	}

	set width(value) {
		this._width = this._height = value;
	}

	set height(value) {
		this._width = this._height = value;
	}
}

let useIt = function (rc) {
	// [1]
	let width = rc._width; // breaks the pattern
	rc.height = 10; // breaks the pattern
	console.log(`Expected area of ${10 * width}, ` + `got ${rc.area}`);
};

let rc = new Rectangle(2, 3);

useIt(rc);

let sq = new Square(5);

useIt(sq);
```

---
