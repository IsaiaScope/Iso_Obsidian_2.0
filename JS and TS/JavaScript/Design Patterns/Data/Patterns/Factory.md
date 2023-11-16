# Factory

> A component responsible solely for the wholesale (not piecewise) creation of objects.

## Motivation

- Object creation logic becomes too convoluted
- Initializer is not descriptive
  - Name is always _ init_
  - Cannot overload with same sets of arguments with different names
  - Can turn into 'optional parameter hell'
- Wholesale object creation (non-piecewise, unlike Builder) can be outsourced to
  - A separate method (Factory Method)
  - That may exist in a separate class (Factory)
  - Can create hierarchy of factories with Abstract Factory

---

## Summary

- A factory method is a static method that creates objects
- A factory is any entity that can take care of object creation
- A factory can be external or reside inside the object as an inner class
- Hierarchies of factories can be used to create related objects

---

## Example

_Factory method_ is a method for manufacturing as new object, and it gives you a certain benefits, in addition to being able to be very explicit about the naming of the method itself and the names of arguments
_PointFactory_ is just a class or object that takes the responsibility to create a class/object of a particular type with some flexibility

```js
CoordinateSystem = {
	CARTESIAN: 0,

	POLAR: 1,
};

class Point {
	constructor(x, y) {
		this.x = x;

		this.y = y;
	}

	// constructor(a, b, cs=CoordinateSystem.CARTESIAN)
	// {
	//   switch (cs)
	//   {
	//     case CoordinateSystem.CARTESIAN:
	//       this.x = a;
	//       this.y = b;
	//       break;
	//     case CoordinateSystem.POLAR:
	//       this.x = a * Math.cos(b);
	//       this.y = a * Math.sin(b);
	//       break;
	//   }
	//
	//
	// steps to add a new system
	//
	// 1. augment CoordinateSystem
	//
	// 2. change ctor
	// }

	static newCartesianPoint(x, y) {
		return new Point(x, y);
	}

	static newPolarPoint(rho, theta) {
		return new Point(rho * Math.cos(theta), rho * Math.sin(theta));
	}

	static get factory() {
		return new PointFactory();
	}
}

class PointFactory {
	// not necessarily static

	newCartesianPoint(x, y) {
		return new Point(x, y);
	}

	static newPolarPoint(rho, theta) {
		return new Point(rho * Math.cos(theta), rho * Math.sin(theta));
	}
}

let p1 = new Point(2, 3, CoordinateSystem.CARTESIAN);

console.log(p1);

// Point → PointFactory

let p2 = PointFactory.newPolarPoint(5, Math.PI / 2);

console.log(p2);

// this line will not work if newCartesianPoint is static!

let p3 = Point.factory.newCartesianPoint(2, 3);

console.log(p3);
```

---

## Example (Abstract Factory)

_Abstract Factory_ is a creational design pattern that lets you produce families of related objects without specifying their concrete classes.

```js
const readline = require("readline");

let rl = readline.createInterface({
	input: process.stdin,

	output: process.stdout,
});

class HotDrink {
	consume() {}
}

class Tea extends HotDrink {
	consume() {
		console.log("This tea is nice with lemon!");
	}
}

class Coffee extends HotDrink {
	consume() {
		console.log(`This coffee is delicious!`);
	}
}

class HotDrinkFactory {
	prepare(amount) {
		/* abstract */
	}
}

class TeaFactory extends HotDrinkFactory {
	prepare(amount) {
		console.log(`Grind some beans, boil water, pour ${amount}ml`);

		return new Tea();
	}
}

class CoffeeFactory extends HotDrinkFactory {
	prepare(amount) {
		console.log(`Put in tea bag, boil water, pour ${amount}ml`);

		return new Coffee();
	}
}

let AvailableDrink = Object.freeze({
	coffee: CoffeeFactory,

	tea: TeaFactory,
});

class HotDrinkMachine {
	constructor() {
		this.factories = {};

		for (let drink in AvailableDrink) {
			this.factories[drink] = new AvailableDrink[drink]();
		}
	}

	makeDrink(type) {
		switch (type) {
			case "tea":
				return new TeaFactory().prepare(200);
			case "coffee":
				return new CoffeeFactory().prepare(50);
			default:
				throw new Error(`Don't know how to make ${type}`);
		}
	}

	interact(consumer) {
		rl.question(
			"Please specify drink and amount " + "(e.g., tea 50): ",
			(answer) => {
				let parts = answer.split(" ");
				let name = parts[0];
				let amount = parseInt(parts[1]);
				let d = this.factories[name].prepare(amount);
				rl.close();
				consumer(d);
			}
		);
	}
}

let machine = new HotDrinkMachine();

// rl.question('which drink? ', function(answer)
// {
//   let drink = machine.makeDrink(answer);
//   drink.consume();
//   rl.close();
// });

machine.interact(function (drink) {
	drink.consume();
});
```

---
