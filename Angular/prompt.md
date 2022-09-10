####  create new Project
```console
ng new <projectName>
```
---
#### upgrade ==local== angular cli version
```console
ng update @angular/core @angular/cli
```
---
#### Generate ==component==
```console
ng g c </path/componetName> --skip-tests --inline-style --inline-template --flat
```
---
#### Create module to ==manage lazy-loading==
-  u can add --flat to avoid folder creation
- the following snippets create the entire component .ts .html .scss .specs 
- also routing.module.ts and module.ts
- adds in app.module the route automatically and assign routeName to routePath and the module to the declaration
```console
ng g m <filePath/folderName> --route <routeName> --module app.module 
```
---
#### Create ==module==
-  create just module.ts and append that module to app.module declaration
- automatically when new component is generated in the same folder are added to this module
```console
ng g m <filePath/moduleName> --module app.module --flat
```

