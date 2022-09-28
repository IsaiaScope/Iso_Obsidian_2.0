#### ==Jest==
- JS testing
- https://github.com/facebook/jest
- https://jest-bot.github.io/jest/docs/getting-started.html
```console
npm i --save-dev jest
```
---
#### ==NODEMON==
- refresh automatically the server when a file is modified then again u don't have to press Crtl+c e restart again the server with  node fileName.js
- in package.json => scrips => start: nodemone nomeFile.js
```console
npm i --save-dev nodemon
```
---
#### ==Express==
```console
npm i --save express
```
---
#### ==Body-parser==
```console
npm i --save body-parser
```
---
#### ==ejs pug express-handlebars==
All three are libraries to send HTML pages back with dynamic data
- pug => great library to create HTML server-side to send to user, implement feature like extend, quite hard
- ejs => better one u could write JS directly in the template, reusable template are called partial 
- express-handlebars => good because logic is always in JS file, really easy this library aim to write way less logic in the template 
```console
npm i --save ejs pug express-hadlebars
```
---
#### ==Sql==
All three are libraries to send HTML pages back with dynamic data
- install SQL server and workbench
- help SQL connection, without using queries and using modals and JS objs to access and CRUD the db
```console
npm i --save mysql2
npm i --save sequelize

```
---