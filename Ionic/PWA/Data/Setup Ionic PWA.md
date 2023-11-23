# Setup Ionic PWA

## Generate Splash Screens and Icons IOS & ANDROID & PWA

- [GitHub capacitor-assets](https://github.com/ionic-team/capacitor-assets)
  - Check for `flags`
- [Ionic Doc - splash-screens-and-icons](https://capacitorjs.com/docs/next/guides/splash-screens-and-icons)
- [You Tube Tutorial](https://youtu.be/K7ghUiXLef8?si=MISUSRlohbGpViok&t=10425)

---

Make sure to have `@capacitor/assets` inside `devDependencies`

```bash
npm install --save-dev @capacitor/assets
```

- Create an `assets` or `resources` folder on root level or add as follow

  - `--assetPath <path>` - Path to the assets directory for your project. By default will check `assets` and `resources` directories, in that order.
  - indie this folder add _logo.png/svg_, _logo-dark.png/svg_

```bash
npx @capacitor/assets generate --iconBackgroundColor '#ffffff' --iconBackgroundColorDark '#000000' --splashBackgroundColor '#ffffff' --splashBackgroundColorDark '#000000'
```

The result is a folder in the root project called icons, where images for the pwa are hold, and _the manifest is automatically update_

---
