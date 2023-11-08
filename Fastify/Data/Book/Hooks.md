# Hooks

contrary to several other frameworks that have the concept of middleware, Fastify is based on hooks

Before going into their details, though, we need to introduce another concept that makes everything even possible. Namely, we need to learn about the *lifecycle* of Fastify applications.

## Lifecycle

The lifecycle depends on the architecture of the framework. Two well-known architectures are usually used to develop a web framework: 
- *Middleware-based architecture*: Thanks to its more manageable learning curve, this is the most known lifecycle architecture, but time has proved it to have some significant drawbacks. When working with this architecture, an application developer must care about the order of declaration of the middleware functions since it influences the order of execution at runtime. Of course, it can be hard to track the execution order in bigger and more complex applications across multiple modules and files. Moreover, writing reusable code might be more challenging because every moving part is more tidily coupled. As a final note, every middleware function will be executed at every cycle, whether needed or not. 
- *Hook-based architecture*: Fastify, unlike some other famous frameworks, implements a hook- based one. This kind of architecture was a precise design decision from day one since a hook system is more scalable and easier to maintain in the long run. As a nice bonus, since a hook only executes if needed, it usually leads to faster applications! However, it is worth mentioning that it is also harder to master. As we already briefly mentioned, we have different and specific components to deal with in a hook-based architecture.

When dealing with web frameworks, there are usually at least two main lifecycles:

- *The application lifecycle*: This lifecycle is in charge of the different phases of the application server execution. It mainly deals with the server boot sequence and shutdown. We can attach “global” functionalities that are shared between every route and plugin. Moreover, we can act at a specific moment of the lifecycle execution, adding a proper hook function. The most common actions we perform are after the server is started or before it shuts down.
- *The request/reply lifecycle*: The request/response phase is the core of every client-server application. Almost the entirety of the execution time is spent inside this sole sequence. For this reason, this lifecycle usually has way more phases and therefore hooks we can add to it. The most common ones are content parsers, serializers, and authorization hooks.

### NOTE

Fastify emits a specific event every time it advances to the next phase. Furthermore, these phases follow a rigid and well-defined execution order. Knowing it enables us to add functionality at a specific point during the boot or the execution of our application.

---

## The onRoute hook

The onRoute hook event is triggered every time a route is added to the Fastify instance. This callback is a synchronous function that takes one argument, commonly called routeOptions. This argument is a mutable object reference to the route declaration object itself, and we can use it to modify route properties.

