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

---

### The onRequest hook

As the name implies, the onRequest hook is triggered every time there is an incoming request. Since it is the first event on the execution list, the body request is always null because body parsing doesn’t happen yet. The hook function is asynchronous

---

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

---

### The preValidation hook

We can use the preValidation hook to change the incoming payload before it is validated. Since the parsing has already happened, we finally have access to the body request, which we can modify directly.

```js
const Fastify = require("fastify");

const app = Fastify({ logger: true });
app.addHook("preValidation", async (request, _reply) => {
	request.body = { ...request.body, preValidation: "added" };
});
app.post("/", (request, _reply) => {
	request.log.info(request.body);
	return "done";
});

app.listen({ port: 3000 }).catch((err) => {
	app.log.error(err);
	process.exit();
});
```

The example adds a simple preValidation hook that modifies the parsed body object. Inside the hook, we use the spread operator to add a property to the body, and then we assign the new value to the request.body property again.

---

### The preSerialization hook

Since we are dealing with the response payload, preSerialization has a similar signature to the preParsing hook. It accepts a request object, a reply object, and a third payload parameter. We can use its return value to change or replace the response object before serializing and sending it to the clients.

There are two essential things to remember about this hook:

- _It is not called if the payload argument is string, Buffer, stream, or null_
- _If we change the payload, it will be changed for every response, including errored ones_

The following pre-serialization.cjs example shows how we can add this hook to the Fastify instance:

```js
const Fastify = require("fastify");

const app = Fastify({ logger: true });
app.addHook("preSerialization", async (request, reply, payload) => {
	return { ...payload, preSerialization: "added" };
});
app.get("/", (request, _reply) => {
	return { foo: "bar" };
});

app.listen({ port: 3000 }).catch((err) => {
	app.log.error(err);
	process.exit();
});
```

Inside the hook, we use the spread operator to copy the original payload and then add a new property. At this point, it is just a matter of returning this newly created object to modify the body before returning it to the client.

---

### The onSend hook

The onSend hook is the last hook invoked before replying to the client. Contrary to preSerialization, the onSend hook receives a payload that is already serialized. Moreover, it is always called, no matter the type of response payload. Even if it is harder to do, we can use this hook to change our response too, but this time we have to return one of string, Buffer, stream, or null. Finally, the signature is identical to preSerialization.

Let’s make an example in the on-send.cjs snippet with the most straightforward payload, a string:

```js
const Fastify = require("fastify");

const app = Fastify({ logger: true });
app.addHook("onSend", async (request, reply, payload) => {
	const newPayload = payload.replace("foo", "onSend");
	return newPayload;
});
app.get("/", (request, _reply) => {
	return { foo: "bar" };
});

app.listen({ port: 3000 }).catch((err) => {
	app.log.error(err);
	process.exit();
});
```

Since the payload is already serialized as a string, we can use the replace method to modify it before returning it to the client.

---

### The onResponse hook

The onResponse hook is the last hook of the request-reply lifecycle. This callback is called after the reply has already been sent to the client. Therefore, we can’t change the payload anymore. However, we can use it to perform additional logic, such as calling external services or collecting metrics

---

### The onError hook

This hook _is triggered only when the server sends an error as the payload to the client. It runs after customErrorHandler if provided or after the default one integrated into Fastify_. Its primary use is to do additional logging or to modify the reply headers. We should keep the error intact and avoid calling reply.send directly. The latter will result in the same error we encountered trying to do the same inside the onResponse hook. The snippet shown in the on-error.cjs example makes it easier to understand:

```js
const Fastify = require("fastify");

const app = Fastify({ logger: true });
app.addHook("onError", async (request, _reply, error) => {
	// [1]
	request.log.info(`Hi from onError hook: ${error.message}`);
});
app.get("/foo", async (_request, _reply) => {
	return new Error("foo"); // [2]
});
app.get("/bar", async (_request, _reply) => {
	throw new Error("bar"); // [3]
});

app.listen({ port: 3000 }).catch((err) => {
	app.log.error(err);
	process.exit();
});
```

First, we define an onError hook at [1] that logs the incoming error message. We don’t want to change the error object to return any value from this hook, as we already said. So then, we define two routes: /foo ([2]) returns an error while /bar ([3]) throws an error.

---

### The onTimeout hook

It depends on the connectionTimeout option, whose default value is 0. Therefore, onTimeout will be called if we pass a custom connectionTimeout value to the Fastify factory. In on-timeout.cjs, we use this hook to monitor the requests that time out. Since it is only executed when the connection socket is hung up, we can’t send any data to the client:

```js
const Fastify = require("fastify");
const { promisify } = require("util");

const wait = promisify(setTimeout);
const app = Fastify({ logger: true, connectionTimeout: 1000 }); // [1]
app.addHook("onTimeout", async (request, reply) => {
	request.log.info("The connection is closed."); // [2]
});

app.get("/", async (_request, _reply) => {
	await wait(5000); // [3]
	return "";
});

app.listen({ port: 3000 }).catch((err) => {
	app.log.error(err);
	process.exit();
});
```

At [1], we pass the connectionTimeout option to the Fastify factory, setting its value to 1 second. Then we add an onTimeout hook that prints to logs every time a connection is closed ([2]). Finally, we add a route that waits for 5 seconds before responding to the client ([3]).

The connection was closed by the server, and the client received an empty response.

---

Replying from a hook

Besides throwing an error, there is another way of early exiting from the request phase execution flow at any point. Terminating the chain is just a matter of sending the reply from a hook: this will prevent everything that comes after the current hook from being executed. For example, this can be useful when implementing authentication and authorization logic.

**However, there are some quirks when dealing with asynchronous hooks. For example, after calling reply.send to respond from a hook, we need to return the reply object to signal that we are replying from the current hook**.

The reply-from-hook.cjs example will make everything clear:

```js
const Fastify = require("fastify");

async function isAuthorized() {
	return false;
}

const app = Fastify({ logger: true });

app.addHook("preParsing", async (request, reply) => {
	const authorized = await isAuthorized(request); // [1]
	if (!authorized) {
		reply.code(401);
		reply.send("Unauthorized"); // [2]
		return reply; // [3]
	}
});

app.listen({ port: 3000 }).catch((err) => {
	app.log.error(err);
	process.exit();
});
```

We check whether the current user is authorized to access the resource [1]. Then, when the user misses the correct permissions, we reply from the hook directly [2] and return the reply object to signal it [3]. We are sure that the hook chain will stop its execution here, and the user will receive the 'Unauthorized' message.

---

## Route-level hooks

Until now, we declared our hooks at the application level. However, as we mentioned previously in this chapter, request/response hooks can also be declared on a route level. Thus, as we can see in route- level-hooks.cjs, we can use the route definition to add as many hooks as we like, allowing us to perform actions only for a specific route:

```js
const Fastify = require("fastify");

const app = Fastify({ logger: true });

app.route({
	method: "GET",
	url: "/",
	onRequest: async (request, reply) => {
		request.log.info("onRequest hook");
	},
	onResponse: async (request, reply) => {
		request.log.info("onResponse hook");
	},
	preParsing: async (request, reply) => {
		request.log.info("preParsing hook");
	},
	preValidation: async (request, reply) => {
		request.log.info("preValidation hook");
	},
	preHandler: async (request, reply) => {
		request.log.info("preHandler hook");
	},
	preSerialization: async (request, reply, payload) => {
		request.log.info("preSerialization hook");
		return payload;
	},
	onSend: [
		async (request, reply, payload) => {
			request.log.info("onSend hook 1");
			return payload;
		},
		async (request, reply, payload) => {
			request.log.info("onSend hook 2");
			return payload;
		},
	], // [1]
	onError: async (request, reply, error) => {},
	onTimeout: async (request, reply) => {},
	handler: function (request, reply) {
		reply.send({ foo: "bar" });
	},
});

app.listen({ port: 3000 }).catch((err) => {
	app.log.error(err);
	process.exit();
});
```

_If we need to declare more than one hook for each category, we can define an array of hooks instead_ ([1]). As a final note, it is worth mentioning that these hooks are always executed as last in the chain.

---

