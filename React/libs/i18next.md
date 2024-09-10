---
tags: 
- i18next
---

# i18next

## NOTE

- middleware must be `./src` folder to work
- i18nconfig.js root folder
- cliente component use the hook
  - `import { useTranslation } from 'react-i18next';`
- server component
  - `const { t, resources } = await initTranslations(locale, ['common', 'home']);` a custom function for calling the json and select just Namespaces you need
- For managing the routes we wrap the the request into middleware file with `i18nRouter(request, i18nConfig)`
- We can pass the pecific Namespaces to child component of a main one with the provider `TranslationsProvider`, to _note_ thats just Namespaces used are passed.
- default name space is `common`

## Main Consideration

Even using next the implementation involve `react-i18next` that under the hood is using `i18next`. So other next library make possible for example manage cookies for keeping user language preference and other simple adjustment.

## Start implementation

### Tutorial for react-i18next

- [react-i18next](https://i18nexus.com/tutorials/nextjs/react-i18next)
- [You Tube Guide for react-i18next](https://www.youtube.com/watch?v=J8tnD2BWY28&t=542s)
- [next-i18n-router](https://github.com/i18nexus/next-i18n-router)
- [Next DOC](https://nextjs.org/docs/app/building-your-application/routing/internationalization)

### Create Translations

- [i18nexus](https://app.i18nexus.com) (Site for translations and where pull json form)
  Scripts to pull json and update type safe into code
- [YT: type safe video guide](https://youtu.be/GLIas4DH3Ww?si=HNX_qCgYpr_gm38d&t=79 "https://youtu.be/GLIas4DH3Ww?si=HNX_qCgYpr_gm38d&t=79")

```shell
// package.json
"update:translations": "i18nexus pull",
"create:translations:type": "i18next-resources-for-ts interface -i ./locales/en -o ./src/types/resources.d.ts"
```

---
