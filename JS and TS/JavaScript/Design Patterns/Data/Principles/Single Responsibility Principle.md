# Single Responsibility Principle

The idea is that each of your classes (or modules, units of code, functions, etc) should responsible only one functionality. Why ?

Because if you want to change your code, you should first have a good reason, then you should know where is that single point where that reason applies

## Separation of Concerns

The principle is simple: don’t write your program as one solid block, instead, break up the code into chunks that are finalized tiny pieces of the system each able to complete a simple distinct job.

## Examples

_Journal class_ [1] is just used for creating and manipulate the data and just that, so it's responsible for one functionality. If you want to add persistence logic like a saving method or other logic in general you should create a new class to deliver it (see _PersistenceManager class_ [2]). The data object is created and written in a file text

```js
const fs = require("fs");

class Journal {
	// [1]
	constructor() {
		this.entries = {};
	}

	addEntry(text) {
		let c = ++Journal.count;

		let entry = `${c}: ${text}`;

		this.entries[c] = entry;

		return c;
	}

	removeEntry(index) {
		delete this.entries[index];
	}

	toString() {
		return Object.values(this.entries).join("\n");
	}
}

Journal.count = 0;

class PersistenceManager {
	// [2]
	preprocess(j) {
		//
	}

	saveToFile(journal, filename) {
		fs.writeFileSync(filename, journal.toString());
	}
}

let j = new Journal();

j.addEntry("I cried today.");

j.addEntry("I ate a bug.");

j.addEntry("I ate a bee.");

console.log(j.toString());

let p = new PersistenceManager();

let filename = "C:/Users/isaia.riva/Desktop/DesignPattern/journal.txt";

p.saveToFile(j, filename);
```

---
