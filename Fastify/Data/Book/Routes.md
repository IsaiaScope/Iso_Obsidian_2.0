# Routes

- [MDN HTTP >> Messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages.)

## Synchronous and asynchronous handlers

In this example, we are simulating two async calls to a readDb function. As you can imagine, adding more and more asynchronous I/O such as files read or database accesses could make the source quickly unreadable, with the danger of falling into callback hell (you can read more about this at http://callbackhell.com/).

You can rewrite the previous example, using the second syntax to define a route handler, with an async function:

```js
async function asyncHandler(request, reply) {
	const data1 = await readDb();
	const data2 = await readDb();
	return `read from db ${data1} and ${data2}`;
}
```

---

## Reply is a Promise

In an async function handler, it is highly discouraged to call reply.send() to send a response back to the client. Use _return_ keyword

---

## How to reply with errors

Generally, in Fastify, an error can be sent when the handler function is sync or thrown when the handler is async. Let’s put this into practice:

```js
function syncHandler(request, reply) {
	const myErr = new Error("this is a 500 error");
	reply.send(myErr); // [1]
}

async function ayncHandler(request, reply) {
	const myErr = new Error("this is a 500 error");
	throw myErr; // [2]
}

async function ayncHandlerCatched(request, reply) {
	try {
		await ayncHandler(request, reply);
	} catch (err) {
		// [3]
		this.log.error(err);
		reply.status(200);
		return { success: false };
	}
}
```

As you can see, at first sight, the differences are minimal: in [1], the send method accepts a Node.js Error object with a custom message. The [2] example is quite similar, but we are throwing the error. The [3] example shows how you can manage your errors with a try/catch block and choose to reply with a 200 HTTP success in any case!

---

## The error handler

- respect hierarchy

```js
async function errorTrigger(request, reply) {
	throw new Error("ops");
}
app.register(async function plugin(pluginInstance) {
	pluginInstance.setErrorHandler(function (error, request, reply) {
		request.log.error(error, "an error happened");
		reply.status(503).send({ ok: false });
	});
	pluginInstance.get("/customError", errorTrigger); // [1]
});
app.get("/defaultError", errorTrigger); // [2]
```

Declaring API endpoints and managing the errors 69 We have defined a bad route handle, errorTrigger, that will always throw an Error. Then, we registered two routes:

- The GET /customError [1] route is inside a plugin, so it is in a new Fastify context.
- The root application instance registers the GET /defaultError [2] route instead.

We set pluginInstance.setErrorHandler, so all the routes registered inside that plugin and its children’s contexts will use your custom error handler function during the plugin creation. Meanwhile, the app’s routes will use the default error handler because we didn’t customize it. At this stage, making an HTTP request to those endpoints will give us two different outputs, as expected:

- The GET /customError route triggers the error, and it is managed by the custom error handler, so the output will be {"ok":false}.
- The GET /defaultError endpoint replies with the Fastify default JSON format that was shown at the beginning of this section.

It is not over yet! Fastify implements an outstanding granularity for most of the features it supports. This means that you can set a custom error handler for every route!

```js
app.get("/routeError", {
	handler: errorTrigger,
	errorHandler: async function (error, request, reply) {
		request.log.error(error, "a route error happened");
		return { routeFail: false };
	},
});
```

- [[Differences between the async and sync handlers.png]]

### The 404 handler

Fastify provides a way to configure a 404 handler. It is like a typical route handler, and it exposes the same interfaces and async or sync logic:

```js
app.register(
	async function plugin(instance, opts) {
		instance.setNotFoundHandler(function html404(request, reply) {
			reply.type("application/html").send(niceHtmlPage);
		});
	},
	{ prefix: "/site" } // [1]
);

app.setNotFoundHandler(function custom404(request, reply) {
	reply.send({ not: "found" });
});
```

In this example, we have set the custom404 root 404 handler and the plugin instance html404. This can be useful when your server manages multiple contents, such as a static website that shows a cute and funny HTML page when an non-existent page is requested, or shows a JSON when a missing API is queried.

The previous code example tells Fastify to search for the handler to execute into the plugin instance when the requested URL starts with the /site string. If Fastify doesn’t find a match in this context, it will use the Not Found handler of that context. So, for example, let’s consider the following URLs:

- The http://localhost:8080/site/foo URL will be served by the html404 handler
- The http://localhost:8080/foo URL will be served by the custom404 instead

The prefix parameter (marked as [1] in the previous code block) is mandatory to set multiple 404 handlers; otherwise, Fastify will not start the server, because it doesn’t know when to execute which one, and it will trigger a startup error.

---

## Router application tuning

- The trailing slash

Fastify thinks that the /foo and /foo/ URLs are different, and you can register them and let them reply to two completely different outputs:

```js
app.get("/foo", function (request, reply) {
	reply.send("plain foo");
});
app.get("/foo/", function (request, reply) {
	reply.send("foo with trailin slash");
});
```

- Case-insensitive URLs

  - Another common issue you could face is having to support both the /fooBar and /foobar URLs as a single endpoint (note the case of the B character). As per the trailing slash example, Fastify will manage these routes as two distinct items; in fact, you can register both routes with two different handler functions
  - The caseSensitive option will instruct the router to match all your endpoints in lowercase

- Rewrite URL

This feature adds the possibility of changing the HTTP’s requested URL before the routing takes place:

```js
const app = fastify({
	rewriteUrl: function rewriteUrl(rawRequest) {
		if (rawRequest.url.startsWith("/api")) {
			return rawRequest.url;
		}
		return `/public/${rawRequest.url}`;
	},
});
```

The rewriteUrl parameter accepts an input sync function that must return a string. The returned line will be set as the request URL, and it will be used during the routing process. Note that the function argument is the standard http.IncomingMessage class and not the Fastify Request component.
This technique could be useful as a URL expander or to avoid redirecting the client with the 302 HTTP response status code.

### The URL’s pathname

If you are struggling while choosing whether your endpoint should be named /fast-car or /fast_car, you should know that a hyphen is broadly used for web page URLs, whereas the underscore is used for API endpoints.

---

## The body

The request’s body can be read through the request.body property. Fastify handles two input content types:

- The application/json produces a JavaScript object as a body value
- The text/plain produces a string that will be set as a request.body value

Note that the request payload will be read for the POST, PUT, PATCH, OPTIONS, and DELETE HTTP methods. The GET and HEAD ones don’t parse the body, as per the HTTP/1.1 specification.

Fastify sets a length limit to the payload to protect the application from Denial-of-Service (DOS) attacks, sending a huge payload to block your server in the parsing phase. When a client hits the default 1-megabyte limit, it receives a 413 - Request body is too large error response. For example, this could be an unwanted behavior during an image upload. So, you should customize the default body size limit by setting the options as follows:

```js
const app = fastify({
	// [1]
	bodyLimit: 1024, // 1 KB
});
app.post(
	"/",
	{
		// [2]
		bodyLimit: 2097152, // 2 MB
	},
	handler
);
```

The [1] configuration defines the maximum length for every route without a custom limit, such as route [2].

---

## Printing the routes tree

```js
app.ready().then(function started() {
	console.log(app.printPlugins()); // [1]
	console.log(app.printRoutes({ commonPrefix: false, includeHooks: true })); // [2]
});
```

The printPlugins() method of [1] returns a string with a tree representation containing all the encapsulated contexts and loading times. The output gives you a complete overview of all the plugins created and the nesting level. Instead, the printRoutes() method of [2] shows us the application’s routes list

Fastify internals
The output shows the bound \_after string. You can simply ignore this string output because it is an internal Fastify behavior, and it does not give us information about our application.

---


