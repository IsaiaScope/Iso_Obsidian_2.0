# Setup Ionic PWA

## Generate Splash Screens and Icons IOS & ANDROID

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
npx @capacitor/assets generate --iconBackgroundColor '#ffffff' --iconBackgroundColorDark '#000000' --splashBackgroundColor '#ffffff' --splashBackgroundColorDark '#000000'  --ios --android
```

The result is a folder in the root project called icons, where images for the pwa are hold, and _the manifest is automatically update_

---

```html
<head>
	<meta name="viewport" content="width=device-width,initial-scale=1" />
	<title>My Awesome App</title>
	<meta name="description" content="My Awesome App description" />

	<link rel="icon" href="/favicon.ico" />

	<link rel="apple-touch-icon" href="/apple-touch-icon.png" sizes="180x180" />

	<link rel="mask-icon" href="/mask-icon.svg" color="#FFFFFF" />

	<meta name="theme-color" content="#ffffff" />
</head>
```

(https://developer.apple.com/library/archive/documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html)

(https://developer.apple.com/library/archive/documentation/AppleApplications/Reference/SafariWebContent/pinnedTabs/pinnedTabs.html)

(https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta/name/theme-color)

https://favicon.inbrowser.app/tools/favicon-generator --- la svolta
https://realfavicongenerator.net/
https://vite-pwa-org.netlify.app/assets-generator/

(https://www.leereamsnyder.com/favicons-in-2021)
(https://dev.to/masakudamatsu/favicon-nightmare-how-to-maintain-sanity-3al7)
(https://en.wikipedia.org/wiki/Favicon)

# Register Service Worker

https://vite-pwa-org.netlify.app/guide/register-service-worker

do not create any file vite do everything itself
injectRegister: 'script'

# Manifest Props

(https://developer.mozilla.org/en-US/docs/Web/Manifest)

## Search Engines

You **must** add a `robots.txt` file to allow search engines to crawl all your application pages, just add `robots.txt` to the `public` folder on your application:

robots.txt - public folrder
User-agent: *
Allow: /


## Service Worker Strategies
As we mention in [Configuring vite-plugin-pwa] section, `vite-plugin-pwa` plugin will use `workbox-build` node library to generate your service worker. There are 2 available strategies, `generateSW` and `injectManifest`:

- `generateSW`: the `vite-plugin-pwa` will generate the service worker for you, you don't need to write the code for the service worker
- `injectManifest`: the `vite-plugin-pwa` plugin will compile your custom service worker and inject its precache manifest

To configure the service worker strategy, use the `strategies`' plugin option with `generateSW` (**default strategy**) or `injectManifest` value.

You can find more information about the strategies in the [generateSW](https://vite-pwa-org.netlify.app/workbox/generate-sw) or [injectManifest](https://vite-pwa-org.netlify.app/workbox/inject-manifest) `Workbox` sections.



