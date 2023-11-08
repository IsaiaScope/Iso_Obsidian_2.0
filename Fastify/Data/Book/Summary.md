# Summary

## Best Practice

- No CallBack, Async code prefered
- A bound this *context* 
	- When dealing with Fastify functionalities that have an explicitly bound this value, as in the case of the onReady hook, it is essential to *use the old function syntax instead of the arrow function one*. Using the latter will prevent binding, making it impossible to access the instance and custom data added to it.

---

## Errors

- When you cough an Error, Fastify is unaware that an error has been caught in your handler. In this case, you must set the HTTP response status code; otherwise, it will be 200 – Success by default.

---

## Plugins

- during the plugin’s registration and thanks to its robust encapsulation, Fastify creates a new instance with a child context

