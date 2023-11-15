# The register instance method

```js
.register(async function plugin(instance, pluginOptions), options)
```

But what is the register return value? It is a thenable Fastify instance, and it brings two significant benefits to the table:

- It adds the ability to chain instance method calls.
- If needed, we can use _await_ when calling register

```js
// register-await.mjs
const Fastify = require("fastify");

const fp = require("fastify-plugin");

async function boot() {
	// [1]
	async function plugin1(fastify, options) {
		fastify.decorate("plugin1Decorator", "plugin1");
	}
	async function plugin2(fastify, options) {
		fastify.decorate("plugin2Decorator", "plugin2");
	}
	async function plugin3(fastify, options) {
		fastify.decorate("plugin3Decorator", "plugin3");
	}

	const app = Fastify({ logger: true });
	await app // [2]
		.register(fp(plugin1))
		.register(fp(plugin2));

	console.log("plugin1Decorator", app.hasDecorator("plugin1Decorator"));
	console.log("plugin2Decorator", app.hasDecorator("plugin2Decorator"));

	app.register(fp(plugin3)); // [3]

	console.log("plugin3Decorator", app.hasDecorator("plugin3Decorator"));

	await app.ready();
	console.log("app ready");
	console.log("plugin3Decorator", app.hasDecorator("plugin3Decorator"));
}

boot();
```

```console
$ node register-await.mjs
plugin1Decorator true # [4]
plugin2Decorator true
plugin3Decorator false # [5]
app ready
plugin3Decorator true # [6]
```

Since this example is quite long and complex, let’s break it into smaller chunks:

- We declare three plugins [1], each adding one decorator to the Fastify instance
- We use fastify-plugin to decorate the root instance and to register these plugins ([2] and [3]), as we learned in the previous section
- Even if our plugins are identical, we can see that the results of console.log are different; the two plugins registered with the await operator ([2]) have already decorated the root instance ([4])
- _Conversely, the last one registered without await ([3]) adds its decorator only after the ready event promise has been resolved ([5] and [6])_
  At this point, it should be clear that if we also want the third decorator to be available before the application is fully ready, it is sufficient to add the await operator on [3]! As a final note, **we can say that we don’t need to wait when registering our plugins for most cases**. The only exception might be when we need access to something that the plugin did during the loading. For example, we have a plugin that connects to an external source, and we want to be sure that it is connected before going on with the boot sequence
