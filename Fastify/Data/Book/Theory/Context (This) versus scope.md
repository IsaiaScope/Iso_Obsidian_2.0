# Context (This) versus scope

Firstly let’s take a look at the definitions of both terms. The _context_ indicates the current value of the implicit ‘_this_’ method variable. The _scope_ is a set of rules that manages the visibility of a variable from a function point of view. In the Fastify community, these two terms are used interchangeably and refer to the Fastify instance we are currently working with. For this reason, in this book, we will use both words, meaning the same thing.

```js
// encapsulation.cjs
const Fastify = require("fastify");
const app = Fastify({ logger: true });
app.decorate("root", "hello from the root instance."); // [1]
app.register(async function myPlugin(fastify, _options) {
	console.log("myPlugin -- ", fastify.root);
	fastify.decorate("myPlugin", "hello from myPlugin."); // [2]
	console.log("myPlugin -- ", fastify.myPlugin);
});
app.ready().then(() => {
	console.log("root -- ", app.root); // [3]
	console.log("root -- ", app.myPlugin);
});
```

```console
$ node encapsulation.cjs
myPlugin -- hello from the root instance.
myPlugin -- hello from myPlugin.
root -- hello from the root instance.
root -- undefined
```

Firstly, we decorate the root instance ([1]), adding a string to it. Then, inside myPlugin, we print the root decorated value and add a new property to the Fastify instance. In the plugin definition body, we log both values in the console to ensure they are set ([2]). Finally, we can see that after the Fastify application is ready, at the root level, we can only access the value we added outside of our plugin ([3]). But what happened here? In both cases, we used the .decorate method to add our value to the instance. Why are both values visible in myPlugin but only the root one visible at the top level? This is the intended behavior, and it happens thanks to encapsulation – _Fastify creates a new context every time it enters a new plugin. We call these new contexts child contexts. A child context inherits only from the parent contexts, and everything added inside a child context will not be visible to its parent or its siblings’ contexts_. The parent-child annidation level is infinite, and we can have contexts that are children to their parents and parents to their children.

The entities that are affected by scoping are:

- Decorators
- Hooks
- Plugins
- Routes

---

## Encapsulation

In real-world applications, more complex scenarios with several child and grandchild contexts are widespread. We can use the following diagram to examine a more complex example:

![[encapsulation.png]]

We have a root Fastify instance that registers two root plugins. Every root plugin creates a new child context where we can again declare and register as many plugins as we want. That’s it – we can have infinite nesting for our plugins, and every level depth will create a new encapsulated context.

### NOTE

_If we need to share context between siblings or alter a parent context, we can still do it_. This comes in handy for more complex plugins, such as ones that handle database connections.

```js
// skip-override.cjs
const Fastify = require("fastify");
async function myPlugin(fastify, _options) {
	console.log("myPlugin -- ", fastify.root);
	fastify.decorate("myPlugin", "hello from myPlugin."); //[2]
	console.log("myPlugin -- ", fastify.myPlugin);
}
myPlugin[Symbol.for("skip-override")] = true; // [1]
const app = Fastify({ logger: true });
app.decorate("root", "hello from the root instance.");
app.register(myPlugin);
app.ready().then(() => {
	console.log("root -- ", app.root);
	console.log("root -- ", app.myPlugin); //[3]
});
```

There is only one major change in this code snippet from the previous one, which is that we use Symbol.for('skip-override') to prevent Fastify from creating a new context ([1]). This alone is enough to have the root decorated with the fastify.myPlugin variable ([2]). We can see that the decorator is also accessible from the outer scope ([3]); here is the output:

```console
$ node skip-override.cjs
myPlugin -- hello from the root instance.
myPlugin -- hello from myPlugin.
root -- hello from the root instance.
root -- hello from myPlugin.
```

Instead of using just the skip-override property as a string, Fastify uses Symbol.for to hide it and avoid name collisions. [MDN Symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)

#### fastify-plugin

Using skip-override is perfectly fine, but there is a better way to control encapsulation behavior. Like many things in the Fastify world, the fastify-plugin module is nothing more than a function that wraps a plugin, adds some metadata to it, and returns it.

- [fastify-plugin](https://www.npmjs.com/package/fastify-plugin)

The _fp naming_ convention In the Fastify community, it is common to import the fastify-plugin package as fp because it is a short yet meaningful variable name. In this book, we will use this convention every time we deal with it.

---
