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
#### ==Sql & Sequelize==

 ![[Sql & Sequelize.png]]
 
```console
npm i --save mysql2 sequelize
```
---
#### ==MongoDB Mongoose==
```console
npm i --save mongodb mongoose
```
---
#### ==Session==
- help managing session on the server
```console
npm i --save express-session
```

- to do not store the session non server memory, that'd be good idea use DB to store the session it self; if u are working with mongoDB the next package could be usefull
```console
npm i --save connect-mongodb-session
```
---
#### ==BcryptJS==
```console
npm i --save bcryptjs
```
---
#### ==CSURF==
![[CSRF.png]]
```console
npm i --save csurf
```
---
#### ==Connect Flash==
![[Flash.png]]
```console
npm i --save connect-flash
```
---
#### ==Send Mails==
- sendgrid => free tier 100 mail per day 
- it's very complicated manage email server so good idea is to get third party package
```console
npm i --save nodemailer nodemailer-sendgrid-transport
```
---
#### ==express-validator & validator==
- useful to validate data like email format and more
```console
npm i --save express-validator
```
- other solution could be
```console
npm i --save validator
```
---
#### ==Multer==
- useful to manage file like img upload to the server
```console
npm i --save multer
```
---
#### ==pdfkit==
- useful to create custom pdf on the server and respond with that
```console
npm i --save pdfkit
```
---
#### ==Stripe==
- useful to manage payments
```console
npm i --save stripe
```
---
#### ==JWT==
- useful to manage jwt
```console
npm i --save jsonwebtoken
```
---