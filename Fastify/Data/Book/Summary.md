# Summary

## Best Practice

- No CallBack, Async code prefered

---

## Errors

- When you cough an Error, Fastify is unaware that an error has been caught in your handler. In this case, you must set the HTTP response status code; otherwise, it will be 200 – Success by default.

---

## Plugins

- during the plugin’s registration and thanks to its robust encapsulation, Fastify creates a new instance with a child context

