# Command

> An object which represents an instruction to perform a particular action. Contains all the information necessary for the action to be taken.

The command pattern allows encapsulation of a request as an object. This transformation lets you pass requests as a method arguments, delay or queue a request’s execution, and support undoable operations.

## Motivation

- Ordinary statements are perishable
  - Cannot undo member assignment
  - Cannot directly serialize a sequence of actions (calls)
- Want an object that represents an operation
  - person should change its age to value 22
  - car should do explode()
- Uses: GUI commands, multi-level undo/redo, macro recording and more!

---

## Summary

- Encapsulate all details of an operation in a separate object
- Define instruction for applying the command (either in the command itself, or elsewhere)
- Optionally define instructions for undoing the command
- Can create composite commands (a.k.a. macros)

---

## Example

Bank account example

```js
class BankAccount {
	constructor(balance = 0) {
		this.balance = balance;
	}

	deposit(amount) {
		this.balance += amount;
		console.log(`Deposited ${amount}, balance is now ${this.balance}`);
	}

	withdraw(amount) {
		if (this.balance - amount >= BankAccount.overdraftLimit) {
			this.balance -= amount;
			console.log(`Withdrew ${amount}, balance is now ${this.balance}`);
			return true;
		}
		return false;
	}

	toString() {
		return `Balance: ${this.balance}`;
	}
}
BankAccount.overdraftLimit = -500;

let Action = Object.freeze({
	deposit: 1,
	withdraw: 2,
});

class BankAccountCommand {
	constructor(account, action, amount) {
		this.account = account;
		this.action = action;
		this.amount = amount;
		this.succeeded = false;
	}

	call() {
		switch (this.action) {
			case Action.deposit:
				this.account.deposit(this.amount);
				this.succeeded = true;
				break;
			case Action.withdraw:
				this.succeeded = this.account.withdraw(this.amount);
				break;
		}
	}

	undo() {
		if (!this.succeeded) return;
		switch (this.action) {
			case Action.deposit:
				this.account.withdraw(this.amount);
				break;
			case Action.withdraw:
				this.account.deposit(this.amount);
				break;
		}
	}
}

let ba = new BankAccount(100);

let cmd = new BankAccountCommand(ba, Action.deposit, 50);
cmd.call();
console.log(ba.toString());

console.log("Performing undo:");
cmd.undo();
console.log(ba.toString());
```

---
