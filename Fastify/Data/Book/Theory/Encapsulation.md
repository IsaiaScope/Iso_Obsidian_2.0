# Encapsulation

Besides showing how onRequest works, the following on-request.cjs snippet also shows how the hook encapsulation works (as we already said, the encapsulation is valid for every other hook as well):

```js
const Fastify = require("fastify");

const app = Fastify({ logger: true });
app.addHook("onRequest", async (request, reply) => {
	// [1]
	request.log.info("Hi from the top-level onRequest hook.");
});
app.register(async function plugin1(instance, options) {
	instance.addHook("onRequest", async (request, reply) => {
		// [2]
		request.log.info("Hi from the child-level onRequest hook.");
	});

	instance.get("/child-level", async (_request, _reply) => {
		// [3]
		return "child-level";
	});
});
app.get("/top-level", async (_request, _reply) => {
	// [4]
	return "top-level";
});

app.listen({ port: 3000 }).catch((err) => {
	app.log.error(err);
	process.exit();
});
```

The first thing we do is add a top-level onRequest hook ([1]). Then we register a plugin that defines another onRequest hook ([2]) and a GET /child-level route ([3]). Finally, we add another GET route on the /top-level path ([4]).

- First of all, let’s get the /child-level route:

```
$ curl localhost:3000/child-level
child-level
```

We can see that curl was able to get the route response correctly. Switching the terminal window again to the one that is running the server, we can check the logs:

```
{"level":30,"time":1636444712019,"pid":30137,"hostname":"localhost", "reqId":"req-1","req":{"method":"GET","url":"/child-level","hostname": "localhost:3000","remoteAddress":"127.0.0.1","remotePort":56903}, "msg":"incoming request"}

{"level":30,"time":1636444712021,"pid":30137,"hostname":" localhost ", "reqId":"req-1","msg":"Hi from the top-level onRequest hook."}

{"level":30,"time":1636444712023,"pid":30137,"hostname":" localhost ", "reqId":"req-1","msg":"Hi from the child-level onRequest hook."}

{"level":30,"time":1636444712037,"pid":30137,"hostname":" localhost ", "reqId":"req-1","res":{"statusCode":200},"responseTime" :16.760624945163727,"msg":"request completed"}
```

We can spot immediately that both hooks were triggered during the request-response cycle. Moreover, we can also see the order of the invocations: first, "Hi from the top-level onRequest hook", and then "Hi from the child-level onRequest hook".

- To ensure that hook encapsulation is working as expected, let’s call the /top-level route. We can switch again to the terminal where curl ran and type the following command:

```
$ curl localhost:3000/top-level
top-level
```

Now coming back to the server log output terminal, we can see the following:

```
{"level":30,"time":1636444957207,"pid":30137,"hostname":" localhost","reqId":"req-2","req":{"method":"GET","url":"/ top-level","hostname":"localhost:3000","remoteAddress":"127.0.0.1", "remotePort":56910},"msg":"incoming request"}

{"level":30,"time":1636444957208,"pid":30137,"hostname": " localhost ","reqId":"req-2","msg":"Hi from the top-level onRequest hook."}

{"level":30,"time":1636444957210,"pid":30137,"hostname":" localhost ","reqId":"req-2","res":{"statusCode":200},"responseTime": 1.8535420298576355,"msg":"request completed"}
```

This time, the logs show that only the top-level hook was triggered. This is an expected behavior, and we can use it to add hooks scoped to specific plugins only.

---