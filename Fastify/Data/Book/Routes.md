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


