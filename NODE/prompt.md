#### NODE Version
```console
node -v
```
---
#### Launch File
```console
node nomeFile.js
```
---
#### App Initialization
- create package.json and other staff like node module folder!
```console
npm init
```
---
#### Random string
```console
node
```
```console
require('crypto').randomBytes(64).toString('hex')
```
---
#### Remove a package
- remember to specify the scope --save, --save-dev, -g 
```console
npm uninstall --save <package_name> 
npm un <package_name>
```
---
#### Update a package
- remember  -g  (between update and p. name) if package is globally installed
```console
npm update <package_name> 
```
---
#### Update a package
- This command removes "extraneous" packages. If a package name is provided, then only packages matching one of the supplied names are removed. Extraneous packages are those present in the `node_modules` folder that are not listed as any package's dependency list.

```console
npm prune [[<@scope>/]<pkg>...]
```