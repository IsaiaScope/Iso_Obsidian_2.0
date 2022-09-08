####  create new Project
```console
ng new <projectName>
```
---
#### Generate ==component==
```console
ng g c </path/componetName> --skip-tests --inline-style --inline-template --flat
```
---
#### upgrade ==local== angular cli version
```console
ng update @angular/core @angular/cli
```
---
#### Create modules to ==manage lazy-loading==
-  u can add --flat to avoid folder creation
- the following snippets create the entire component .ts .html .scss .specs 
- adds in app.module the route automatically => routeName
```console
ng g m <filePath/folderName> --route <routeName> --module app.module 
```
