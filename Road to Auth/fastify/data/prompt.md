##### Init new project
```console
npm init -y
```
- **fastify-swagger** => give us open Api documentation for Api
- **uuid** => for generating random id
```console
npm i fastify fastify-swagger uuid
```
---

```js
// dependencies
yarn add @prisma/client fastify fastify-zod zod fastify—jwt
// devDependencies
yarn add typescript @types/node —dev
// Initialise prisma
npx prisma init —datasource—provider postgresql
// Migrate the schema
npx prisma migrate dev —name init
```