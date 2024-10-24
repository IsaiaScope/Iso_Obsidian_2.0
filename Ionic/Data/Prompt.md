---
tags: 
- Ionic
---

# Prompt

[ionicframework](https://ionicframework.com/)

## CLI

```bash
npm i -g @ionic/cli
```

---

## Init React App

- The fallowing command create a new folder with the project's folders inside

```
ionic start <app-name> blank --type=react
```

---

## Work with Native

[capacitor](https://capacitorjs.com/)

### Adding Platform

- Build the project because, is possible to serve just the dist on platform like Android Studio

```
ionic build
```

#### Android Folder

- Add android folder to the project where the native code for android is hold

```
ionic cap add android
```

#### IOS Folder

- Add ios folder to the project where the native code for ios is hold

```
ionic cap add ios
```

---

### Synchronise/Update Native Folder

Copy the dist folder into android and ios folder creating native code for both

```
ionic cap sync
```

---

### Developing Android (Android Studio)

For opening android studio from command line we need capacitor CLI. But we don't have the CLI installed globally; just locally in our project under `devDependencies` described by `@capacitor/cli`, So for accessing those commands we can use:

```
node_modules/.bin/cap open android
```

_node_modules/.bin/_ is where che CLI is hold, _cap open android_ is the command to launch where _cap_ is the main command served by the CLI

On the other hand we can create a local script inside package.json and inside a local script we're able to access local CLI commands

```json
 "scripts": {
    "open:as": "cap open android"
  },
```

Then launch in console

```bash
npm run open:as
```

### See Android Studio Available Device

List of devices on android studio, sometime show devices that doesn't exist so make sure to import that specific device in android studio manually because also in android studio there are sometimes devices that you never import/create

```
ionic cap run android --list
```

### Live reloading during developing

Run `ipconfig` under `IPv4 Address` to assign a value to `--public-host`

You dont need to open Android studio to run this command

```
ionic cap run android --livereload --external --public-host=192.168.6.201
```

---

### Developing IOS (X Code)

A mac is needed for developing

Commands are equals for android's one just replace _android_ with _ios_

---
