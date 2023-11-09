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

