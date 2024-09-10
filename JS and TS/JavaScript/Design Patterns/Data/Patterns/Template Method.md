# Template Method

> Allows us to define the 'skeleton' of the algorithm, with concrete implementations defined in subclasses.

The template pattern allows defining the skeleton of an algorithm in the superclass but lets subclasses override specific steps of the algorithm without changing its structure.

## Motivation

- Algorithms can be decomposed into common parts + specifics
- Strategy pattern does this through composition
  - High-level algorithm uses an interface
  - Concrete implementations implement the interface
- Template Method does the same thing through inheritance
  - Overall algorithm makes use of empty ('abstract') members
  - Inheritors override those members
  - Parent template method invoked

---

## Summary

- Define an algorithm at a high level
- Define constituent parts as empty methods/properties
- Inherit the algorithm class, providing necessary overrides

---

## Example

```js
class Game {
	constructor(numberOfPlayers) {
		this.numberOfPlayers = numberOfPlayers;
		this.currentPlayer = 0;
	}

	run() {
		this.start();
		while (!this.haveWinner) {
			this.takeTurn();
		}
		console.log(`Player ${this.winningPlayer} wins.`);
	}

	start() {}
	get haveWinner() {}
	takeTurn() {}
	get winningPlayer() {}
}

class Chess extends Game {
	constructor() {
		super(2);
		this.maxTurns = 10;
		this.turn = 1;
	}

	start() {
		console.log(
			`Starting a game of chess with ${this.numberOfPlayers} players.`
		);
	}

	get haveWinner() {
		return this.turn === this.maxTurns;
	}

	takeTurn() {
		console.log(`Turn ${this.turn++} taken by player ${this.currentPlayer}.`);
		this.currentPlayer = (this.currentPlayer + 1) % this.numberOfPlayers;
	}

	get winningPlayer() {
		return this.currentPlayer;
	}
}

let chess = new Chess();
chess.run();
```

---
