# Façade

> Provides a simple, easy to understand/user interface over a large and sophisticated body of code.

The facade pattern provides a simplified interface to a set of interfaces in a subsystem. Facade defines a higher-level interface that makes the subsystem easier to use

## Motivation

- Balancing complexity and presentation/usability
- Typical home
  - Many subsystems (electrical, sanitation)
  - Complex internal structure (e.g., floor layers)
  - End user is not exposed to internals
- Same with software!
  - Many systems working to provide flexibility, but...
  - API consumers want it to 'just work'

---

## Summary

- Build a Façade to provide a simplified API over a set of classes
- May wish to (optionally) expose internals through the façade
- May allow users to 'escalate' to use more complex APIs if they need to

---

## Example

In this example, we are creating a simple interface *Cart* that abstracts all the complexity from several subsystems as *Discount, Shipping,* and *Fees*.

```js
class Cart {
	constructor() {
		this.discount = new Discount();

		this.shipping = new Shipping();

		this.fees = new Fees();
	}

	calc(price) {
		price = this.discount.calc(price);

		price = this.fees.calc(price);

		price += this.shipping.calc();

		return price;
	}
}

class Discount {
	calc(value) {
		return value * 0.85;
	}
}

class Shipping {
	calc() {
		return 500;
	}
}

class Fees {
	calc(value) {
		return value * 1.1;
	}
}

export default Cart;
```

---
