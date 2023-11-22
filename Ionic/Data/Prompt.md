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

## Create Folders For Native

[capacitor](https://capacitorjs.com/)

- Build the project because, is possible to serve just the dist on platform like Android Studio

```
ionic build
```

### Android Folder

- Add android folder to the project where the native code for android is hold

```
ionic capacitor add android
```

### IOS Folder

- Add ios folder to the project where the native code for ios is hold

```
ionic capacitor add ios
```

---

## Developing Android (Android Studio)

```
cap open android
```

---

## Developing IOS (X Code)

A mac is needed for developing

```
cap open ios
```

---
