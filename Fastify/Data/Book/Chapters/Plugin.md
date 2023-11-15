# Plugins

## What is a plugin?

A Fastify plugin is an essential tool at the disposal of a developer. Every functionality except the root instance of the server should be wrapped in a plugin. Plugins are the key to reusability, code sharing, and achieving proper encapsulation between Fastify instances

Fastify’s root instance will load all registered plugins asynchronously following the registration order during the boot sequence. Furthermore, a plugin can depend on others, and Fastify checks these dependencies and exits the boot sequence with an error if it finds missing ones.

### Creating our first plugin

```js
const Fastify = require("fastify");
const app = Fastify({ logger: true }); // [1]
app.register(async function (fastify, opts) {
	// [2]
	app.log.info("Registering my first plugin.");
});
app
	.ready() // [3]
	.then(() => {
		app.log.info("All plugins are now registered!");
	});
```

### NOTE

Finally, we call the ready method ([3]). This function starts the boot sequence and returns Promise, which will be settled when all plugins have been loaded. For the moment, we don’t need a listening server in our examples, so it is acceptable to call ready instead. Moreover, listen internally awaits for the .ready() event to be dispatched anyway.

No matter which style you use, _plugins are always asynchronous functions_, and every pattern has its error handling

---

## Exploring the options parameter

It is worth mentioning that Fastify reserves three specific options that have a special meaning:

- prefix
- logLevel
- logSerializers

We will cover logLevel and logSerializer in a dedicated chapter. Here, we will focus only on prefix and custom options

```js
app.register(
	async function myPlugin(fastify, options) {
		console.log(options.myplugin.first);
	},
	{ prefix: "v1", myPlugin: { first: "custom option" } }
);
```

Instead of adding our custom properties at the top level of the options parameter, we will group them in a custom key, lowering the chances for future name collisions. Passing an object is fine in most cases, but sometimes, we need more flexibility.

### The prefix option

The prefix option comes in handy here because it allows us to add a namespace to our route declarations.

- Maintaining different versions of our APIs
- Reusing the same plugin and routes definition for various applications, giving another mount point each time

```js
// users-router.cjs
module.exports = async function usersRouter(fastify, _) {
	fastify.register(
		async function routes(child, _options) {
			// [1]
			child.get("/", async (_request, reply) => {
				reply.send(child.users);
			});
			child.post("/", async (request, reply) => {
				// [2]
				const newUser = request.body;
				child.users.push(newUser);
				reply.send(newUser);
			});
		},
		{ prefix: "users" } // [3]
	);
};
```

```js
// users-router-index.cjs
const Fastify = require("fastify");
const usersRouter = require("./users-router.cjs");

const app = Fastify();
app.decorate("users", [
	// [1]
	{
		name: "Sam",
		age: 23,
	},
	{
		name: "Daphne",
		age: 21,
	},
]);

app.register(usersRouter, { prefix: "v1" }); // [2]
app.register(
	async function usersRouterV2(fastify, options) {
		// [3]
		fastify.register(usersRouter); // [4]
		fastify.delete("/users/:name", (request, reply) => {
			// [5]
			const userIndex = fastify.users.findIndex(
				(user) => user.name === request.params.name
			);
			fastify.users.splice(userIndex, 1);
			reply.send();
		});
	},
	{ prefix: "v2" }
);

app.ready().then(() => {
	console.log(app.printRoutes());
}); // [6]
```

First of all, we decorate the Fastify root instance with the users property ([1]); as previously, this will act as our database for this example. On [2], we register our user’s router with the v1 prefix. Then, we register a new inline-declared plugin ([3]), using the v2 namespace (every route added in this plugin will have the v2 namespace). On [4], we register the same user’s routes for a second time, and we also add a newly declared delete route ([5])

Indeed, prefixing route definitions is a compelling feature. It allows us to reuse the same route declarations more than once. It is one of the crucial elements of the reusability of the Fastify plugins

---

## Context (This) versus scope

- [[Context (This) versus scope]]

---

## Order of plugin registration

But how does Fastify know the correct order of plugin registration? Is it even a deterministic process?

_Firstly, it is essential to say that the Fastify boot sequence is asynchronous too. Fastify loads every plugin added with the register method, one by one, respecting the order of the registration. Fastify starts this process only after .listen() or .ready() are called. After that, it waits for all promises to be settled (or for all completed callbacks to be called, if the callback style is used), and then it emits the ready event. If we have already got this far, we can be sure that our application is up and running and ready to receive incoming requests_

Even if Fastify’s boot process is very versatile, it handles almost everything out of the box. The exposed API is small – there are just two methods! The first one is the register method, which we have already used many times. The other one is _after_, and as we will see, it is rarely used because register integrates its functionalities for most of the use cases.

- [[The register instance method]]
- [[The after instance method]]
- [[Handling boot and plugin errors]]

Using the following declaration order will help figure out what the issue is:

- Plugins installed from npmjs.com
- Our plugins
- Decorators The Plugin System and the Boot Process54
- Hooks
- Services, routes, and so on
