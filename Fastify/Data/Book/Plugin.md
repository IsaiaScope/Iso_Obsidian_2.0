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