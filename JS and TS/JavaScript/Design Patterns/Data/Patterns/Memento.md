# Memento

> A token/handle representing the system state. Lets us roll back to the state when the token was generated. May or may not directly expose state information.

The memento pattern allows capture and externalizes an object’s internal state so that the object can be restored to this state later.

## Motivation

- An object or system goes through changes
  - E.g., a bank account gets deposits and withdrawals
- There are different ways of navigating those changes
- One way is to record every change (Command) and teach a command to 'undo' itself
- Another is to simply save snapshots of the system (Memento).

---

## Summary

- Mementos are used to roll back states arbitrarily
- A memento is simply a token/handle class with (typically) no functions of its own
- A memento is not required to expose directly the state(s) to which it reverts the system
- Can be used to implement undo/redo

---

## Example

get a snapshot of the system (Bank Account)

```js
class Memento {
	constructor(balance) {
		this.balance = balance;
	}
}

class BankAccount {
	constructor(balance = 0) {
		this.balance = balance;
	}

	deposit(amount) {
		this.balance += amount;

		return new Memento(this.balance);
	}

	restore(m) {
		this.balance = m.balance;
	}

	toString() {
		return `Balance: ${this.balance}`;
	}
}

let ba = new BankAccount(100);
let m1 = ba.deposit(50);
let m2 = ba.deposit(25);
console.log(ba.toString());

// restore to m1
ba.restore(m1);
console.log(ba.toString());

// restore to m2
ba.restore(m2);
console.log(ba.toString());
```

---

## Example (Undo and Redo)

get a snapshot of the system (Bank Account) adding an undo method

```js
class Memento {
	constructor(balance) {
		this.balance = balance;
	}
}

class BankAccount {
	constructor(balance = 0) {
		this.balance = balance;

		this.changes = [new Memento(balance)];

		this.current = 0;
	}

	deposit(amount) {
		this.balance += amount;

		let m = new Memento(this.balance);

		this.changes.push(m);

		this.current++;

		return m;
	}

	restore(m) {
		if (m) {
			this.balance = m.balance;

			this.changes.push(m);

			this.current = this.changes.count - 1;
		}
	}

	undo() {
		if (this.current > 0) {
			let m = this.changes[--this.current];

			this.balance = m.balance;

			return m;
		}

		return null;
	}

	redo() {
		if (this.current + 1 < this.changes.length) {
			let m = this.changes[++this.current];

			this.balance = m.balance;

			return m;
		}

		return null;
	}

	toString() {
		return `Balance: $${this.balance}`;
	}
}

let ba = new BankAccount(100);

ba.deposit(50);
ba.deposit(25);
console.log(ba.toString());

ba.undo();
console.log(`Undo 1: ${ba.toString()}`);

ba.undo();
console.log(`Undo 2: ${ba.toString()}`);

ba.redo();
console.log(`Redo 2: ${ba.toString()}`);
```

---
