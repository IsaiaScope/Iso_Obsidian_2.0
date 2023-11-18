# Validation and Serialization

Fastify is secure and fast, but that doesn’t protect it from misuse. This chapter will teach you how to implement secure endpoints with input validation and make them faster with the serialization process.

You will learn how to use and configure Fastify’s components in order to control and adapt the default setup to your application logic. To find solutions for and prevent the most common security attacks, such as code injection or sensitive data exposure. The answer is declaring the expected input and output data format for every route. Therefore, the validation and serialization processes have been introduced into the framework by design:

- [[Validation and Serialization.png]]

## NOTE

- _The Validation_ phase happens when the HTTP Request comes into the server. It allows you to approve or deny access to the Business Logic step.
- _The Serialization_ step transforms high-level data produced by the business logic, such as JSON objects or errors, into low-level data (strings or buffers) to reply to the client’s request.
- The next question is: how do you define the information passing through the validation and the response data? Fortunately, the solution is the _JSON Schema_ specification, which is embraced by both Fastify and the web community.

---

## The JSON Schema specification

The JSON Schema standard describes the structure of JSON documents. Therefore, by using a JSON Schema interpreter, it is possible to verify whether a JSON object adapts to a defined structure and act accordingly.

Writing a schema gives you the possibility to apply some automation to your Node.js application:

- Validating JSON objects
- Generating documentation
- Filtering JSON object fields

The following JSON object through a schema:

```json
{ "id": 1, "name": "Foo", "hobbies": ["Soccer", "Scuba"] }
```

The corresponding JSON Schema could assume the following structure:

```json
{
	"$schema": "http://json-schema.org/draft-07/schema#",
	"$id": "http://foo/user",
	"type": "object",
	"properties": {
		"identifier": { "type": "integer" },
		"name": { "type": "string", "maxLength": 50 },
		"hobbies": { "type": "array", "items": { "type": "string" } }
	},
	"required": ["identifier", "name"]
}
```

We expect a type object as input with some properties that we have named and configured as follows:

- The required identifier field must be an integer
- A mandatory name string that cannot be longer than 50 characters
- An optional hobbies string array

In this case, a software interpreter’s output that checks if the input JSON is compliant with the schema would be successful. The same check would fail if the input object doesn’t contain one of the mandatory fields, or if one of the types doesn’t match the schema’s type field.
The _serialization_ is a nice “side effect,” introduced to improve security and performance; we will see how within this section.

- [DOC of json-schema](https://json-schema.org/)

Describing last snippet as you may have noticed, some keywords have the dollar symbol prefix, $. This is special metadata defined in the draft standard. One of the most important and most used ones is the _\$id_ property. It identifies the JSON Schema univocally, and Fastify relies upon it to process the schema objects and reuse them across the application.

The _\$schema_ keyword in the example tells us the JSON document’s format, which is Draft-07. Whenever you see a JSON Schema in these pages, it is implicit that it follows that version due to Fastify’s default setup.

### Compiling a JSON Schema

A JSON Schema is not enough to validate a JSON document. We need to transform the schema into a function that our software can execute. For this reason, it is necessary to use a _compiler_ that does the work.

This improves the application’s security. In fact, the compiler’s implementation has a secure mechanism to block code injection, and you can configure it to reject bad input data, such as strings that are too long.

#### Fastify’s compilers

Fastify has two compilers by default:

- The Validator Compiler: Compiles the JSON Schema to validate the request’s input
- The Serializer Compiler: Compiles the response’s JSON Schema to serialize the application’s data

These compilers are basically Node.js modules that take JSON Schema as input and give us back a function.

![[Fastify’s JSON Schema compilation workflow.png]] ^036f7f

As you can see, there are two distinct processes:

- The Route initialization, where the schemas are compiled during the startup phase.
- The request through the Request Lifecycle, which uses the compiled functions stored in the route’s context.

---

## Understanding the validation process

The validation process in Fastify follows the same logic to validate the incoming HTTP request parts. This business logic comprises two main steps, as we saw in [[Validation and Serialization#^036f7f | Validation and Serialization]]:

- The schema compilation executed by the _Validator Compiler _
- The validation execution

### The validator compiler

Fastify doesn’t implement a JSON Schema interpreter itself. Still, it has integrated the _Ajv_ module to accomplish the validation process. The Ajv integration into Fastify is implemented to keep it as fast as possible and support the encapsulation as well. You will always be able to change the default settings and provide a new JSON Schema interprete

```js
app.post(
	"/echo/:myInteger",
	{
		schema: {
			params: jsonSchemaPathParams,
			body: jsonSchemaBody,
			querystring: jsonSchemaQuery,
			headers: jsonSchemaHeaders,
		},
	},
	function handler(request, reply) {
		reply.send(request.body);
	}
);
```

All these properties are optional, so you can choose freely which HTTP part has to be validated. The schemas must be provided during the route declaration:

You are done! Now, whenever you start the application, the schemas will be compiled by the default validator compiler. The generated functions will be stored in the route’s context, so every HTTP request that hits the /echo/:myinteger endpoint will execute the validation process.

### Validation execution

Fastify applies the HTTP request part’s validation during the request lifecycle: after executing the preValidation hooks and before the preHandler hooks.

The purpose of this validation is to check the input format and to produce one of these actions:

- Pass: Validates the HTTP request part successfully
- Deny: Throws an error when an HTTP request part’s validation fails
- Append error: When an HTTP request part’s validation fails and continues the process successfully – configuring the attachValidation route option

This process is not designed to verify data correctness – for that, you should rely on the preHandler hook.

_route doesn't have the schema route option. By doing so, we skipped the validation phase of the request lifecycle_

The validation process is quite straightforward. Let’s zoom in on this process’ logic, looking at the entire request lifecycle:

![[The validation execution workflow.png]]

Let’s understand the flow diagram step by step:

- The dotted arrow is an HTTP request that has started its lifecycle into the Fastify server and has reached the preValidation hooks step. All will work as expected, and we are ready to start the Validation Execution.
- Every HTTP part is validated if a JSON Schema has been provided during the route’s declaration.
- The validation passes and proceeds to the next step.
- When the validation fails, a particular Error object is thrown, and it will be processed by the _error handler_ configured in the server instance where the route has been registered. Note that the error is suppressed if the attachValidation route option is set. We will look at an example in the Flow control section.
- If all the validations are successful, the lifecycle continues its flow to the preHandler hooks
- The Business Logic dashed box represents the handler execution that has been omitted because the image is specifically focused on validating the execution flow.

These steps happen when the schema option is set into the route definition, as in the previous code snippet in The validator compiler section.

Now we have a complete overview of the entire validation process, from the startup to the server’s runtime. The information provided covers the most common use cases for an application and, thanks to Fastify’s default settings, it is ready to use.

Great applications need great features. This is why we will now focus on the validator compiler customization.

### Customizing the validator compiler

- SEBA BOOK pdf page n.149 to page n. 155
  - Flow control
  - Understanding the Ajv configuration

### The validation error

The validator function is going to throw an error whenever an HTTP part doesn’t match the route’s schema. The route’s context error handler manages the error. Here is a quick example to show how a custom error handler could manage an input validation error in a different way:

```js
"use strict";

const fastify = require("fastify");

const app = fastify({
	logger: true,
	ajv: {
		customOptions: {
			coerceTypes: "array",
			removeAdditional: "all",
		},
	},
});

app.get("/custom-error-handler", {
	handler: echo,
	schema: {
		query: { myId: { type: "integer" } },
	},
});

app.setErrorHandler(function (error, request, reply) {
	if (error.validation) {
		const { validation, validationContext } = error;
		this.log.warn({ validationError: validation });
		const errorMessage = `Validation error on ${validationContext}`;
		reply.status(400).send({ fail: errorMessage });
	} else {
		this.log.error(error);
		reply.status(500).send(error);
	}
});

app.listen({ port: 8080 });

async function echo(request, reply) {
	return {
		params: request.params,
		body: request.body,
		query: request.query,
		headers: request.headers,
	};
}
```

As you can see, when the validation fails, two parameters are appended to the Error object:

- The validationContext property is the HTTP part’s string representation, responsible for generating the error
- The validation field is the raw error object, returned by the validator compiler implementation

If you just need to customize the error message instead, Fastify has an option even for that! The schemaErrorFormatter option accepts a function that must generate the Error object, which will be thrown during the validation process flow. This option can be set in the following ways:

- During the root server initialization
- As the route’s option
- Or on the plugin registration’s instance

Here is a complete overview of the three possibilities in the same order as in the preceding list:

```js
"use strict";

const fastify = require("fastify");

const app = fastify({
	schemaErrorFormatter: function (errors, httpPart) {
		// [1]
		return new Error("root error formatter");
	},
});
app.get("/custom-route-error-formatter", {
	handler: echo,
	schema: { query: { myId: { type: "integer" } } },
	schemaErrorFormatter: function (errors, httpPart) {
		// [2]
		return new Error("route error formatter");
	},
});
app.register(function plugin(instance, opts, next) {
	instance.get("/custom-error-formatter", {
		handler: echo,
		schema: { query: { myId: { type: "integer" } } },
	});
	instance.setSchemaErrorFormatter(function (errors, httpPart) {
		// [3]
		return new Error("plugin error formatter");
	});
	next();
});

app.listen({ port: 8080 });

async function echo(request, reply) {
	return {
		params: request.params,
		body: request.body,
		query: request.query,
		headers: request.headers,
	};
}
```

The setSchemaErrorFormatter input function must be synchronous. It is going to receive the raw errors object returned by the compiled validation function, plus the part of HTTP that is not valid.

---
### Reusing JSON schemas

[[SEBA Book.pdf#page=157&selection=27,0,27,20|SEBA Book, page 157]]

### Retrieving your schemas

[[SEBA Book.pdf#page=159&selection=123,0,123,23|SEBA Book, page 159]]

## Understanding the serialization process

[[SEBA Book.pdf#page=165&selection=113,0,113,39|SEBA Book, page 165]]

### The reply serializer

[[SEBA Book.pdf#page=167&selection=96,0,96,20|SEBA Book, page 167]]

