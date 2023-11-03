# Fastify

## What is Fastify?

Fastify is a Node.js web framework used to build server applications. It helps you develop an HTTP server and create your API in an easy, fast, scalable, and secure way!
This framework focuses on unique aspects that are uncommon in most other web frameworks:

- Improvement of the developer experience: This streamlines their work and promotes a plugin design system. This architecture helps you structure your application in smaller pieces of code and apply good programming patterns such as DRY (Don’t Repeat Yourself), Immutability, and **Divide & Conquer**.
- Comprehensive performance: This framework is built to be the fastest.
- Up to date with the evolution of the Node.js runtime: This includes quick bugfixes and feature delivery.
- Ready to use: Fastify helps you set up the most common issues you may face during the implementation, such as application logging, security concerns, automatic test implementation, and user input parsing.
- Community-driven: Supports and listens to the framework users. The result is a flexible and highly extensible framework that will lead you to create reusable components. These concepts give you the boost to develop a proof of concept (PoC) or large applications faster and faster.

Creating your plugin system takes less time to meet the business need without losing the possibility to create an excellent code base and a performant application.

---

## Fastify’s components

Fastify makes it easier to create and manage an HTTP server and the HTTP request lifecycle, hiding the complexity of the Node.js standard modules. It has two types of components: the **main components** and **utility elements**. The former comprise the framework, and it is mandatory to deal with them to create an application. The latter includes all the features you may use at your convenience to improve the code reusability.
The main components that are going to be the main focus of this book and that we are going to discuss in this chapter are:

- **The root application instance** represents the Fastify API at your disposal. It manages and controls the standard Node.js http.Server class and sets all the endpoints and the default behavior for every request and response.
- A **plugin instance** is a child object of the application instance, which shares the same interface. It isolates itself from other sibling plugins to let you build independent components that can’t modify other contexts.
- The Request object is a wrapper of the standard Node.js http.IncomingMessage that is created for every client’s call. It eases access to the user input and adds functional capabilities, such as logging and client metadata.
- The Reply object is a wrapper of the standard Node.js http.ServerResponse and facilitates sending a response back to the user

The utility components, which will be further discussed in Chapter 4 are:

- **The hook** functions that act, when needed, during the lifecycle of the application or a single request and response
- **The decorators**, which let you augment the features installed by default on the main components, avoiding code duplication
- **The parsers**, which are responsible for the request’s payload conversion to a primitive type

---

## Starting your server

```
npm init
```

```
npm install fastify
```

Create a new _starts.cjs_ - CommonJS (CJS):CommonJS is a module system adopted by Node. js and used on the server-side. CJS allows for the import and export of modules using require and module. exports. This enables modules to be loaded synchronously and provides a simple syntax.

```js
const fastify = require("fastify"); // [1]
const serverOptions = {
	// [2]
	logger: true,
};
const app = fastify(serverOptions); // [3]
app.listen({ port: 8080, host: "0.0.0.0" }).then((address) => {
	// [4]
	// Server is now listening on ${address}
});
```

Calling listen with the _0.0.0.0 host_ will make your server accept any unspecified IPv4 addresses. This configuration is necessary for a Docker container application or in any application that is directly exposed on the internet; otherwise, external clients won’t be able to call your HTTP server.

```
node starts.cjs
```

---

## Lifecycle and hooks overview

Fastify implements two systems that regulate its internal workflow: _the application lifecycle_ and _the request lifecycle_. These two lifecycles trigger a large set of events during the application’s lifetime. Listening to those events will let us customize the data flow around the endpoints or simply add monitoring tools.

- The onRoute event acts when you add an endpoint to the server instance
- The onRegister event is unique as it performs when a new encapsulated context is created
- The onReady event runs when the application is ready to start listening for incoming HTTP requests
- The onClose event executes when the server is stopping

_All these events are Fastify’s hooks. More specifically, a function that runs whenever a specific event happens in the system is a hook_. The hooks that listen for application lifecycle events are called application hooks.

```js
app.addHook("onRoute", function inspector(routeOptions) {
	console.log(routeOptions);
});

app.addHook("onRegister", function inspector(plugin, pluginOptions) {
	console.log("Chapter 2, Plugin System and Boot Process");
});

app.addHook("onReady", function preLoading(done) {
	console.log("onReady");
	done();
});

app.addHook("onClose", function manageClose(done) {
	console.log("onClose");
	done();
});
```

- The _onRoute and the onRegister_ hooks have some object arguments. These types can only manipulate the input object adding side effects. A side effect changes the object’s properties value, causing new behavior of the object itself.
- The _onReady and onClose_ hooks have a callback style function input instead. The done input function can impact the application’s startup because the server will wait for its completion until you call it. In this timeframe, it is possible to load some external data and store it in a local cache. If you call the callback with an error object as the first parameter, done(new Error()), the application will listen, and the error will bubble up, crashing the server startup. So, it’s crucial to load relevant data and manage errors to prevent them from blocking the server

### NOTE

whenever a Fastify interface exposes a done or next argument, you can omit it and provide an async function instead. So, you can write:

```js
app.addHook("onReady", async function preLoading() {
	console.log("async onReady"); // the done argument is gone!
});
```

### Application lifecycle

- [[Application-lifecycle.png]]

At the start of the application, the onRoute and onRegister hooks are executed whenever a new route or a new encapsulated context is created (we will discuss the encapsulated context by the end of this chapter, in the Adding a basic plugin instance section). The dashed lines in Figure 1.1 mean that these hooks’ functions are run synchronously and are not awaited before the server starts up. When the application is loaded, the onReady hooks queue is performed, and the server will start listening if there are no errors during this startup phase. Only after the application is up and running will it be able to receive stop events. These events will start the closing stage, during which the onClose hooks’ queue will be executed before stopping the server. The closing phase will be discussed in the Shutting down the application section.

### The request lifecycle

The request triggers these events in order during its handling:

1. _onRequest_: The server receives an HTTP request and routes it to a valid endpoint. Now, the request is ready to be processed.
2. _preParsing_ happens before the evaluation of the request’s body payload.
3. The _preValidation hook_ runs before applying JSON Schema validation to the request’s parts. Schema validation is an essential step of every route because it protects you from a malicious request payload that aims to leak your system data or attack your server. Chapter 5 discusses this core aspect further and will show some harmful attacks.
4. _preHandler_ executes before the endpoint handler.
5. _preSerialization_ takes action before the response payload transformation to a String, a Buffer, or a Stream, in order to be sent to the client.
6. _onError_ is executed only if an error happens during the request lifecycle.
7. _onSend_ is the last chance to manipulate the response payload before sending it to the client.
8. _onResponse_ runs after the HTTP request has been served. We will see some examples later on. I hope you have enjoyed the spoilers! But first, we must

---

## Server options

[DOC of Server options](https://fastify.dev/docs/latest/Reference/Server/)

---

## Application instance methods

- _app.route(options[, handler])_ adds a new endpoint to the server.
- _app.register(plugin)_ adds plugins to the server instance, creating a new server context if needed. This method provides Fastify with encapsulation, which will be covered in Chapter 2.
- _app.ready([callback])_ loads all the applications without listening for the HTTP request.
- _app.listen(port|options [,host, callback])_ starts the server and loads the application.
- _app.close([callback])_ turns off the server and starts the closing flow. This generates the possibility to close all the pending connections to a database or to complete running tasks.
- _app.inject(options[, callback])_ loads the server until it reaches the ready status and submits a mock HTTP request. You will learn about this method in Chapter 9.

```js
app.route({
	url: "/hello",
	method: "GET",
	handler: function myHandler(request, reply) {
		reply.send("world");
	},
});
```

### Shorthand declaration

All the HTTP methods, including GET, POST, PUT, HEAD, DELETE, OPTIONS, and PATCH, support this declaration. You need to call the correlated function accordingly: app.post(), app.put(), app.head(), and so on.

```js
app.get(url, handlerFunction); // [1]
app.get(url, {
	// [2]
	handler: handlerFunction, // other options
});
app.get(url, [options], handlerFunction); // [3]
```

---

## The handler

Using an arrow function will prevent you from getting the function context. Without the context, you don’t have the possibility to use the _this_ keyword to access the application instance. The arrow function syntax may not be a good choice because it can cause you to lose a great non-functional feature

```js
function business(request, reply) {
	// `this` is the Fastify application instance
	reply.send({ helloFrom: this.server.address() });
}
app.get("/server", business);
```

_The business logic can be synchronous or asynchronous_: Fastify supports both interfaces, but you must be aware of how to manage the reply object in your source code. In both situations, the handler should _never call reply.send(payload)more than once_

### Shorthand declaration

To ease this task (avoid multiple reply), Fastify supports the return even in the synchronous function handler. So, we will be able to rewrite our first section example as the following:

```js
function business(request, reply) {
	return { helloFrom: this.server.address() };
}
```

### NOTE

1. The async handler function may completely avoid calling the reply.send method instead. It can return the payload directly.
2. we have updated a synchronous interface to an async interface, updating how we manage the response payload accordingly. The async functions that do not execute the send method can be beneficial to reuse handlers in other handler functions, as in the following example:

```js
async function foo(request, reply) {
	return { one: 1 };
}
async function bar(request, reply) {
	const oneResponse = await foo(request, reply);
	return { one: oneResponse, two: 2 };
}
app.get("/foo", foo);
app.get("/bar", bar);
```

---

