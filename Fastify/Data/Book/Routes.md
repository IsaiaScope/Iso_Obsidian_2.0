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
is a function that is executed whenever an Error object or a JSON is thrown or sent; this means that the error handler is the same regardless of the implementation of the route.

```js

```