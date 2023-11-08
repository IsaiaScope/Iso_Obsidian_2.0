# Hooks

contrary to several other frameworks that have the concept of middleware, Fastify is based on hooks

Before going into their details, though, we need to introduce another concept that makes everything even possible. Namely, we need to learn about the _lifecycle_ of Fastify applications.

## Lifecycle

The lifecycle depends on the architecture of the framework. Two well-known architectures are usually used to develop a web framework:

- _Middleware-based architecture_: Thanks to its more manageable learning curve, this is the most known lifecycle architecture, but time has proved it to have some significant drawbacks. When working with this architecture, an application developer must care about the order of declaration of the middleware functions since it influences the order of execution at runtime. Of course, it can be hard to track the execution order in bigger and more complex applications across multiple modules and files. Moreover, writing reusable code might be more challenging because every moving part is more tidily coupled. As a final note, every middleware function will be executed at every cycle, whether needed or not.
- _Hook-based architecture_: Fastify, unlike some other famous frameworks, implements a hook- based one. This kind of architecture was a precise design decision from day one since a hook system is more scalable and easier to maintain in the long run. As a nice bonus, since a hook only executes if needed, it usually leads to faster applications! However, it is worth mentioning that it is also harder to master. As we already briefly mentioned, we have different and specific components to deal with in a hook-based architecture.

When dealing with web frameworks, there are usually at least two main lifecycles:

- _The application lifecycle_: This lifecycle is in charge of the different phases of the application server execution. It mainly deals with the server boot sequence and shutdown. We can attach “global” functionalities that are shared between every route and plugin. Moreover, we can act at a specific moment of the lifecycle execution, adding a proper hook function. The most common actions we perform are after the server is started or before it shuts down.
- _The request/reply lifecycle_: The request/response phase is the core of every client-server application. Almost the entirety of the execution time is spent inside this sole sequence. For this reason, this lifecycle usually has way more phases and therefore hooks we can add to it. The most common ones are content parsers, serializers, and authorization hooks.

### NOTE

Fastify emits a specific event every time it advances to the next phase. Furthermore, these phases follow a rigid and well-defined execution order. Knowing it enables us to add functionality at a specific point during the boot or the execution of our application.

---

## Route hooks

### The onRoute hook

The onRoute hook event is triggered every time a route is added to the Fastify instance. This callback is a _synchronous_ function that takes one argument, commonly called routeOptions. This argument is a mutable object reference to the route declaration object itself, and we can use it to modify route properties.

_onRoute_ application hook, which listens for every route registered in the app context (and its children contexts). It can mutate the routeOptions object before Fastify instantiates the route endpoint.

```js
// on-route.cjs
const Fastify = require("fastify");
const app = Fastify({ logger: true });

app.addHook("onRoute", (routeOptions) => {
	// [1]
	async function customPreHandler(request, reply) {
		request.log.info("Hi from customPreHandler!");
	}
	app.log.info("Adding custom preHandler to the route.");
	routeOptions.preHandler = [
		...(routeOptions.preHandler ?? []),
		customPreHandler,
	]; // [2]
});
app.route({
	// [3]
	url: "/foo",
	method: "GET",
	schema: {
		200: {
			type: "object",
			properties: {
				foo: {
					type: "string",
				},
			},
		},
	},
	handler: (req, reply) => {
		reply.send({ foo: "bar" });
	},
});

app
	.listen({ port: 3000 })
	.then(() => {
		app.log.info("Application is listening.");
	})
	.catch((err) => {
		app.log.error(err);
		process.exit();
	});
```

After setting up the Fastify instance, we add a new onRoute hook ([1]). The sole purpose of this hook is to add a route-level preHandler hook ([2]) to the route definition, even if there weren’t any previous hooks defined for this route ([3]).

#### NOTE

Here, we can learn two important outcomes that will help us when dealing with route-level hooks:

- The routeOptions object is mutable, and we can change its properties. However, if we want to keep the previous values, we need to explicitly re-add them ([2]).
- Route-level hooks are arrays of hook functions (we will see more about this later in the chapter).

---

### The onRegister hook

The onRegister hook is similar to the previous one regarding how it works, but we can use it when dealing with plugins. In fact, for every registered plugin that creates a new encapsulation context, the onRegister hooks are executed before the registration.

**We can use this hook to discover when a new context is created and add or remove functionality; as we already learned, during the plugin’s registration and thanks to its robust encapsulation, Fastify creates a new instance with a child context. Note that this hook’s callback function won’t be called if the registered plugin is wrapped in fastify-plugin.**

The onRegister hook accepts a synchronous callback

```js
// on-register.cjs
const Fastify = require("fastify");
const fp = require("fastify-plugin");

const app = Fastify({ logger: true });
app.decorate("data", { foo: "bar" }); // [1]
app.addHook("onRegister", (instance, options) => {
	app.log.info({ options });
	instance.data = { ...instance.data }; // [2]
});
app.register(
	async function plugin1(instance, options) {
		instance.data.plugin1 = "hi"; // [3]
		instance.log.info({ data: instance.data });
	},
	{ name: "plugin1" }
);
app.register(
	fp(async function plugin2(instance, options) {
		instance.data.plugin2 = "hi2"; // [4]
		instance.log.info({ data: instance.data });
	}),
	{ name: "plugin2" }
);

app
	.ready()
	.then(() => {
		app.log.info("Application is ready.");
		app.log.info({ data: app.data }); // [5]
	})
	.catch((err) => {
		app.log.error(err);
		process.exit();
	});
```

First, we decorate the top-level Fastify instance with a custom data object ([1]). Then we attach an onRegister hook that logs the options plugin and shallow-copy the data object ([2]). This will effectively create a new object that inherits the foo property, allowing us to have encapsulated the data object. At [3], we register our first plugin that adds the plugin1 property to the object. On the other hand, the second plugin is registered using fastify-plugin ([4]), and therefore Fastify will not trigger our onRegister hook. Here, we modify the data object again, adding the plugin2 property to it.

```console
$ node on-register.cjs
{"level":30,"time":1636192862276,"pid":13381,"hostname":"localhost", "options":{"name":"plugin1"}}

{"level":30,"time":1636192862276,"pid":13381,"hostname":"localhost", "data":{"foo":"bar","plugin1":"hi"}}

{"level":30,"time":1636192862277,"pid":13381,"hostname":"localhost", "data":{"foo":"bar","plugin2":"hi2"}}

{"level":30,"time":1636192862284,"pid":13381,"hostname":"localhost", "msg":"Application is ready."}

{"level":30,"time":1636192862284,"pid":13381,"hostname":"localhost", "data":{"foo":"bar","plugin2":"hi2"}}
```

#### Shallow-copy versus deep copy

Since an object is just a reference to the allocated memory address in JavaScript, we can copy the objects in two different ways. By default, we “shallow-copy” them: if one source object’s property references another object, the copied property will point to the same memory address. We implicitly create a link between the old and new property. If we change it in one place, it is reflected in the other. On the other hand, deep-copying an object means that whenever a property references another object, we will create a new reference and, therefore, a memory allocation. Since deep copying is expensive, all methods and operators included in JavaScript make shallow copies.

---

### onReady hook

The onReady hook is triggered after fastify.ready() is invoked and before the server starts listening. If the call to ready is omitted, then listen will automatically call it, and these hooks will be executed anyway. Since we can define multiple onReady hooks, the server will be ready to accept incoming requests only after all of them are completed

Contrary to the other two hooks we already saw, this one is asynchronous. Therefore, it is crucial to define it as an async function or manually call the done callback to progress with the server boot and code execution. In addition to this, the onReady hooks are invoked with the this value bound to the Fastify instance.

---

### onClose hook

onClose is triggered during the shutdown phase right after fastify.close() is called. Thus, it is handy when plugins need to do something right before stopping the server, such as cleaning database connections. This hook is _asynchronous_ and accepts one argument, the Fastify instance. As usual, when dealing with async functionalities, there is also an optional done callback (the second argument) if the async function isn’t used.

---

## Understanding the request and reply lifecycle

Since they are part of the request/reply cycle, the trigger order of these events is crucial. OnError and onTimeout can be triggered in no specific order at every step in the cycle since an error or a timeout can happen at any point

![[Request and reply lifecycle.png]]

Inside the dotted bubble, we can see the request phase. These callback hooks are called, in top-down order, following the arrow’s direction, before the user-defined handler for the current route.

The dashed bubble contains the reply phase, whose hooks are called after the user-defined route handler. Every hook, at every point, can throw an error or go into the timeout. If it happens, the request will finish in the solid bubble, leaving the normal flow.

### Handling errors inside hooks

```js
fastify.addHook("onResponse", async (request, reply) => {
	throw new Error("Some error");
});
```

Since we have access to the reply object, we can also change the reply’s response code and reply to the client directly from the hook. If we choose not to reply in the case of an error or to reply with an error, then Fastify will call the onError hook.

### The preParsing hook

Declaring a preParsing hook allows us to transform the incoming request payload before it is parsed. This callback is asynchronous and accepts three parameters: Request, Reply, and the payload stream. Again, the body request is null since this hook is triggered before preValidation. Therefore, we must return a stream if we want to modify the incoming payload. Moreover, developers are also in charge of adding and updating the receivedEncodedLength property of the returned value.

The example shows how to change the incoming payload working directly with streams:

```js
// pre-parsing.cjs
const Fastify = require("fastify");
const { Readable } = require("stream");

const app = Fastify({ logger: true });
app.addHook("preParsing", async (request, _reply, payload) => {
	let body = "";
	for await (const chunk of payload) {
		// [1]
		body += chunk;
	}
	request.log.info(JSON.parse(body)); // [2]

	const newPayload = new Readable(); // [3]
	newPayload.receivedEncodedLength = parseInt(
		request.headers["content-length"],
		10
	);
	newPayload.push(JSON.stringify({ changed: "payload" }));
	newPayload.push(null);

	return newPayload;
});
app.post("/", (request, _reply) => {
	request.log.info(request.body); // [4]
	return "done";
});

app.listen({ port: 3000 }).catch((err) => {
	app.log.error(err);
	process.exit();
});
```

At [1], we declare our preParsing hook that consumes the incoming payload stream and creates a body string. We then parse the body ([2]) and log the content to the console. At [3], we create a new Readable stream, assign the correct receivedEncodedLength value, push new content into it, and return it. Finally, we declare a dummy route ([4]) to log the body object.

