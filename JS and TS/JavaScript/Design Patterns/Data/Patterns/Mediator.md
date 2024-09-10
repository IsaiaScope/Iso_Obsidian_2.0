# Mediator

> A component that facilitates communication between other components without them necessarily being aware of each other or having direct (reference) access to each other.

## Motivation

- Components may go in and out of a system at any time
  - Chat room participants
  - Players in an MMORPG
- It makes no sense for them to have direct references to one another
  - Those references may go dead
- Solution: have them all refer to some central component that facilitates communication

---

## Summary

- Create the mediator and have each object in the system refer to it
- Mediator engages in bidirectional communication with its connected components
- Mediator has functions the components can call
- Components have functions the mediator can call

---

## Example (Mediator)

chat room that is an array that keep person inside

```js
class Person {
	constructor(name) {
		this.name = name;
		this.chatLog = [];
	}

	receive(sender, message) {
		let s = `${sender}: '${message}'`;
		console.log(`[${this.name}'s chat session] ${s}`);
		this.chatLog.push(s);
	}

	say(message) {
		this.room.broadcast(this.name, message);
	}

	pm(who, message) {
		this.room.message(this.name, who, message);
	}
}

class ChatRoom {
	constructor() {
		this.people = [];
	}

	broadcast(source, message) {
		for (let p of this.people)
			if (p.name !== source) p.receive(source, message);
	}

	join(p) {
		let joinMsg = `${p.name} joins the chat`;
		this.broadcast("room", joinMsg);
		p.room = this;
		this.people.push(p);
	}

	message(source, destination, message) {
		for (let p of this.people)
			if (p.name === destination) p.receive(source, message);
	}
}

let room = new ChatRoom();

let john = new Person("John");
let jane = new Person("Jane");

room.join(john);
room.join(jane);

john.say("hi room");
jane.say("oh, hey john");

let simon = new Person("Simon");
room.join(simon);
simon.say("hi everyone!");

jane.pm("Simon", "glad you could join us!");
```

---

## Example (Mediator With Events)

We're gonna generate event to subscribe to

```js
class Event {
	constructor() {
		this.handlers = new Map();
		this.count = 0;
	}

	subscribe(handler) {
		this.handlers.set(++this.count, handler);
		return this.count;
	}

	unsubscribe(idx) {
		this.handlers.delete(idx);
	}

	fire(sender, args) {
		this.handlers.forEach(function (v, k) {
			v(sender, args);
		});
	}
}

class PlayerScoredEventArgs {
	constructor(playerName, goalsScoredSoFar) {
		this.playerName = playerName;
		this.goalsScoredSoFar = goalsScoredSoFar;
	}

	print() {
		console.log(
			`${this.playerName} has scored ` + `their ${this.goalsScoredSoFar} goal`
		);
	}
}

class Game {
	constructor() {
		this.events = new Event();
	}
}

class Player {
	constructor(name, game) {
		this.name = name;
		this.game = game;
		this.goalsScored = 0;
	}

	score() {
		this.goalsScored++;
		let args = new PlayerScoredEventArgs(this.name, this.goalsScored);
		this.game.events.fire(this, args);
	}
}

class Coach {
	constructor(game) {
		this.game = game;

		game.events.subscribe(function (sender, args) {
			if (args instanceof PlayerScoredEventArgs && args.goalsScoredSoFar < 3) {
				console.log(`coach says: well done, ${args.playerName}`);
			}
		});
	}
}

let game = new Game();
let player = new Player("Sam", game);
let coach = new Coach(game);

player.score();
player.score();
player.score();
```

---
