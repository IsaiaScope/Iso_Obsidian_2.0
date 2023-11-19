# Plugins

- [fastify-plugin](https://www.npmjs.com/package/fastify-plugin)
  - better way to control encapsulation behavior. Like many things in the Fastify world, the fastify-plugin module is nothing more than a function that wraps a plugin, adds some metadata to it, and returns it
- [fastify-overview](https://github.com/Eomm/fastify-overview)
  - By using the fastify-overview plugin, you will be able to create a graphical layout of your application with all the hooks, decorators, and Fastify-encapsulated contexts highlighted! You should give it a try.
  - fastify-cli
    - command line interface (CLI) helps us start our application
- @fastify/autoload
  - The autoload plugin automatically loads the plugins found in a directory and configures the routes to match the folders’ structure
- @fastify/env
  - plugin, which throws an error whenever it doesn’t find an expected variable.
- @fastify/mongodb
  - plugin. It provides access to a MongoDB database to use on the application’s endpoints
  - @fastify/swagger and  @fastify/swagger-ui plugins.
	  - Documenting a complete list of all the application’s endpoints
-  @fastify/cors plugin
- @fastify/jwt
	- It handles low-level primitives around the tokens and lets us focus only on the logic we need inside our application.
- @fastify/multipart 
	- for file uploads

# Libs

- [find-my-way](https://github.com/delvedor/find-my-way)
  - Fastify uses an external module to implement the router logic called find-my-way
- [header-constraint-strategy](https://github.com/Eomm/header-constraint-strategy)
  - Registering the same URLs
- [qs](https://www.npmjs.com/package/qs)
  - change the default query string parser
- [Ajv](https://www.npmjs.com/package/ajv)
  - Fastify doesn’t implement a JSON Schema interpreter itself. Still, it has integrated the Ajv module
