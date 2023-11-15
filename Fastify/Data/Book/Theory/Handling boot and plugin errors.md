# Handling boot and plugin errors

## Recovery from a boot error

Usually, if an error happens during boot time, something serious prevents our application from starting. In such cases, as we already saw, the best thing Fastify can do is to stop the boot process and notify us about the errors it encountered. However, there are a few cases where we can recover from an error and continue with the boot process. For example, we have an optional plugin to measure application metrics, and we want to start the application even if it is not loading correctly

```js
// error-after.cjs
const Fastify = require("fastify");
// [1]
async function plugin1(fastify, options) {
	throw new Error("Kaboom!");
}
const app = Fastify({ logger: true });
app.register(plugin1).after((err) => {
	if (err) {
		// [2]
		console.log(
			`There was an error loading plugin1: '${err.message}'. Skipping.`
		);
	}
});
app.ready().then(() => {
	console.log("app ready");
});
```

First, we declare a dummy plugin ([1]) that always throws. Then, we register it and use the after method to check for registration errors ([2] ). If any error is found, we log it to the console. Calling after has the effect of “catching” the boot error, and therefore, the boot process will not stop since we handled the unexpected behavior. We can run the snippet to check that it works as expected:

```console
$ node error-after.cjs 
There was an error loading plugin1: 'Kaboom!'. Skipping. 
app ready
```