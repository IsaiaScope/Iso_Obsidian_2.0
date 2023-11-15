# Dependency Inversion Principle

The general idea of this principle is as simple as it is important: High-level modules, which provide complex logic, should be easily reusable and unaffected by changes in low-level modules, which provide utility features. To achieve that, you need to introduce an abstraction that decouples the high-level and low-level modules from each other.

- High-level modules should not depend on low-level modules. Both should depend on abstractions.
- Abstractions should not depend on details. Details should depend on abstractions

An important detail of this definition is, that high-level *and* low-level modules depend on the abstraction. The design principle does not just change the direction of the dependency, as you might have expected when you read its name for the first time. It splits the dependency between the high-level and low-level modules by introducing an abstraction between them. So in the end, you get two dependencies:

- the high-level module depends on the abstraction, and
- the low-level depends on the same abstraction.

## Examples

LOW-LEVEL (STORAGE) [1] is the place where to store the relationship, HIGH-LEVEL (RESEARCH) [2] must not depend on `relationships.data` [3] so we can create an "interface" `RelationshipBrowser` to do the job (connecting HIGH-LEVEL (RESEARCH) and LOW-LEVEL (STORAGE) and provide the functionality `findAllChildrenOf()` without creating a relationship between them). _This create abstraction between levels eliminating dependency_

```js
let Relationship = Object.freeze({
	parent: 0,
	child: 1,
	sibling: 2,
});

class Person {
	constructor(name) {
		this.name = name;
	}
}

// LOW-LEVEL (STORAGE) [1]
class RelationshipBrowser {
	constructor() {
		if (this.constructor.name === "RelationshipBrowser")
			throw new Error("RelationshipBrowser is abstract!");
	}

	findAllChildrenOf(name) {}
}

class Relationships extends RelationshipBrowser {
	constructor() {
		super();
		this.data = [];
	}

	addParentAndChild(parent, child) {
		this.data.push({
			from: parent,
			type: Relationship.parent,
			to: child,
		});
		this.data.push({
			from: child,
			type: Relationship.child,
			to: parent,
		});
	}

	findAllChildrenOf(name) {
		return this.data
			.filter((r) => r.from.name === name && r.type === Relationship.parent)
			.map((r) => r.to);
	}
}

// HIGH-LEVEL (RESEARCH) [2]
class Research {
	// constructor(relationships)
	// {
	//   // problem: direct dependence ↓↓↓↓ on storage mechanic
	//   let relations = relationships.data; [3]
	//   for (let rel of relations.filter(r =>
	//     r.from.name === 'John' &&
	//     r.type === Relationship.parent
	//   ))
	//   {
	//     console.log(`John has a child named ${rel.to.name}`);
	//   }
	// }

	constructor(browser) {
		for (let p of browser.findAllChildrenOf("John")) {
			console.log(`John has a child named ${p.name}`);
		}
	}
}

let parent = new Person("John");
let child1 = new Person("Chris");
let child2 = new Person("Matt");

// low-level module
let rels = new Relationships();
rels.addParentAndChild(parent, child1);
rels.addParentAndChild(parent, child2);

new Research(rels);
```

---
