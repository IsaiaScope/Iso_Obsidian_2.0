# The after instance method

The function argument of the after method is called automatically by Fastify when all plugins added until that point have finished loading. As the only parameter, it defines a callback function with an optional error argument

```js
.after(function (err) {})
```

If we don’t pass the callback, then after will return a thenable object that can be awaited. In this version, awaiting after can be replaced by just awaiting register; the behavior is the same. Because of that, in the after.cjs example, we will see the callback style version of the after method:

```js
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
		.register(fp(plugin2))
		.after((_error) => {
			// [3]
			console.log("plugin1Decorator", app.hasDecorator("plugin1Decorator"));
			console.log("plugin2Decorator", app.hasDecorator("plugin2Decorator"));
		})
		.register(fp(plugin3)); // [4]

	console.log("plugin3Decorator", app.hasDecorator("plugin3Decorator"));

	await app.ready();
	console.log("app ready");
}
boot();
```

We declare and then register the same three plugins from the last examples ([1]). On [2], we start our chain of method calls, adding the after call ([3]) before the last register ([4]). Inside the after callback, we are sure, if the error is null, that the first two plugins are loaded correctly; in fact, the decorators have the correct values. If we run this snippet, we will have the same result as the previous one. Fastify guarantees that all after callbacks will be called before the application’s ready event is dispatched. This method can be helpful if we prefer chaining instance methods but still need control over the boot sequence.

---
