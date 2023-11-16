# Iterator

> An object that facilitates the traversal of a data structure.

## Motivation

- Iteration (traversal) is a core functionality of various data structures
- An iterator is a class that facilitates the traversal
  - Keeps a reference to the current element
  - Knows how to move to a different element
  - Knows when it's done and there are no elements to move to
- JavaScript supports this through:
  - Symbol. iterator member that returns
  - An iterator object with a function called next () that
  - Returns an object containing:
    - The value being iterated
    - The done flag indicating whether iteration is finished
- An iterator object itself can also be made iterable

---

## Summary

---

## Example (Array Backed Properties)

Making Creature and showing max, min and average stats

```js
class Creature {
	constructor() {
		this.stats = new Array(3).fill(10);
	}

	get strength() {
		return this.stats[0];
	}

	set strength(value) {
		this.stats[0] = value;
	}

	get agility() {
		return this.stats[1];
	}

	set agility(value) {
		this.stats[1] = value;
	}

	get intelligence() {
		return this.stats[2];
	}

	set intelligence(value) {
		this.stats[2] = value;
	}

	get sumOfStats() {
		return this.stats.reduce((x, y) => x + y, 0);
	}

	get averageStat() {
		return this.sumOfStats / this.stats.length;
	}

	get maxStat() {
		return Math.max(...this.stats);
	}
}

let creature = new Creature();

creature.strength = 10;

creature.agility = 11;

creature.intelligence = 15;

console.log(
	`Creature has average stat ${creature.averageStat}, ` +
		`max stat = ${creature.maxStat}, ` +
		`sum of stats = ${creature.sumOfStats}.`
);
```

---

## Example (Iterator)

```js
class Stuff {
	constructor() {
		this.a = 11;

		this.b = 22;
	} // default iterator

	[Symbol.iterator]() {
		let i = 0;

		let self = this;

		return {
			next: function () {
				return {
					done: i > 1,

					value: self[i++ === 0 ? "a" : "b"],
				};
			},
		};
	}

	get backwards() {
		let i = 0;

		let self = this;

		return {
			next: function () {
				return {
					done: i > 1,

					value: self[i++ === 0 ? "b" : "a"],
				};
			}, // make iterator iterable

			[Symbol.iterator]: function () {
				return this;
			},
		};
	}
}

let values = [100, 200, 300];

for (let i in values) {
	console.log(`Element at pos ${i} is ${values[i]}`);
}

for (let v of values) {
	console.log(`Value is ${v}`);
}

let stuff = new Stuff();

for (let item of stuff) console.log(`${item}`);

for (let item of stuff.backwards) console.log(`${item}`);
```

---

## Example (Tree Traversal)

```js
class Node {
	constructor(value, left = null, right = null) {
		this.value = value;

		this.left = left;

		this.right = right;

		this.parent = null;

		if (this.left) left.parent = this;

		if (this.right) right.parent = this;
	}
}

function makeInOrderIterator(root) {
	// go to leftmost

	let current = root;

	while (current.left) {
		current = current.left;
	}

	let yieldedStart = false;

	return {
		next: function () {
			if (!yieldedStart) {
				yieldedStart = true;

				return {
					value: current,

					done: false,
				};
			}

			if (current.right) {
				current = current.right;

				while (current.left) {
					current = current.left;
				}

				return {
					value: current,

					done: false,
				};
			} else {
				let p = current.parent;

				while (p && current === p.right) {
					current = p;

					p = p.parent;
				}

				current = p;

				return {
					value: current,

					done: current == null,
				};
			}
		}, // next // this makes the iterator iterable

		[Symbol.iterator]: function () {
			return this;
		},
	};
}

class BinaryTree {
	constructor(rootNode) {
		this.rootNode = rootNode;
	} // assuming only one form of iteration

	[Symbol.iterator]() {
		return makeInOrderIterator(this.rootNode);
	}

	*betterInOrder() {
		function* traverse(current) {
			if (current.left) {
				for (let left of traverse(current.left)) yield left;
			}

			yield current;

			if (current.right) {
				for (let right of traverse(current.right)) yield right;
			}
		}

		for (let node of traverse(this.rootNode)) yield node;
	}

	get inOrder() {
		return makeInOrderIterator(this.rootNode);
	}
}

//   1

//  / \

// 2   3

// in-order:  213

// preorder:  123

// postorder: 231

let root = new Node(1, new Node(2), new Node(3));

// c++ style

let it = makeInOrderIterator(root);

let result = it.next();

while (!result.done) {
	console.log(result.value.value);

	result = it.next();
}

let tree = new BinaryTree(root);

for (let x of tree) console.log(x.value);

console.log([...tree].map((x) => x.value));

console.log([...tree.inOrder].map((x) => x.value));

// a generator is both an iterator and iterable

console.log("using a generator...");

console.log([...tree.betterInOrder()].map((x) => x.value));

for (let x of tree.betterInOrder()) console.log(x.value);
```

---
