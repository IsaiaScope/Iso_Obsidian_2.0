

1. https://ionicframework.com/docs/
2. https://ionic.io/ionicons (lib built in ionic)
3. https://capacitorjs.com/docs/
4. 




1. [CLI](https://ionicframework.com/docs/intro/cli) => to enable following script
2.  Prompt
```
ionic -v
```
```
ionic --help
```
```
ionic start --help
```
```
ionic start <name> <template> [options]
```
```
ionic start quelliDiValencia blank --type=react --capacitor
```
3. Edit package.json and  rm package-lock.json after then npm i
4. install CLI also locally
```
npm i @ionic/cli --save-dev --save-exact
```
5. add Splashscreen.hide()
---
- Run ionic capacitor add to add a native iOS or Android project using Capacitor
- Generate your app icon and splash screens using cordova-res --skip-config --copy
- Explore the Ionic docs for components, tutorials, and more: https://ion.link/docs
- Building an enterprise app? Ionic has Enterprise Support and Features: https://ion.link/enterprise-edition