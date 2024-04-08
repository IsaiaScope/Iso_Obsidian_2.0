# Upgrade shaka-player Fimlbank

## Important

### Filmbank custom side of the build

Filmbank implements a custom version of shaka-player where the [mozilla LanguageMapping lib](https://github.com/mozilla/language-mapping-list) is expanded to cover more languages (custom languages retrived from the backend) and generate custom labels. Those labels are used for subtitle labels inside the switch caption option of the player; just for that. They don't center with subtitles of the video, they are the labels for selecting the specific language from the player option.

## The Update Flow

- [ ] Create a new project pulling the [shaka-player repo](https://github.com/google/shaka-player.git)
- [ ] Move on Tag Version of your interest; do not use branches that end with `.x`, use tagged branches with a specific version that includes also the minor releases as 4.7._11_
- [ ] Check if every prerequisite is satisfied by launching on Linux terminal in the repo folder, if something is missing install everthing is needed

```bash
build/install-linux-prereqs.sh
```

- [ ] Create/Open the _dds-repository-libs-shaka_ with this origin `https://git-codecommit.eu-west-1.amazonaws.com/v1/repos/dds-repository-libs-shaka`, this repo is situated in CodeCommit section on AWS of Filmbank at Repositories List
- [ ] Create new branch in dds-repository-libs-shaka to hold shaka-player version update

- [ ] The custom build involved 3 files on dds-repository-libs-shaka

  - build/dds (folder with custom business logic to add)
  - types/dds (for saying witch js files include and exclude from the build)
    - /ui/language_utils.js (the original from shaka repo is removed)
    - /dds/language_utils.js (our custom one is added)

- [ ] In shaka-player repo project copy `types/dds` and `build/dds`
- [ ] than update `dds/language_utils.js` with `ui/language_utils.js` but make sure to override the part of language mapping; we need to replace `mozilla.LanguageMapping` with `shaka.ui.LanguageMapping` and use that instance instead the mozilla one

```js
  goog.require('shaka.ui.LanguageMapping');
  ....


  static getLanguageName(locale, localization) {
	const language = shaka.util.LanguageUtils.getBase(locale);
    const languageMapping = new shaka.ui.LanguageMapping();
    const languageMap = languageMapping.getMap();

    if (locale in languageMap) {
      return languageMap[locale].nativeName;
    } else if (language in languageMap) {
      return languageMap[language].nativeName +
          ' (' + locale + ')';
    } else {
      return resolve(shaka.ui.Locales.Ids.UNRECOGNIZED_LANGUAGE) +
          ' (' + locale + ')';
    }
  }
```

- [ ]
- [ ] shaka-player.dds

la build la trovi in 'shaka-player/dist/shaka-player.ui.debug'

---

build @complete, per aggiungere file alla build dds-repository-libs-shaka/build/types agggiungere un file li, puoi omettere e aggiungere roba
nel file complete configurare la build
|3|+@ads|
|4|+@cast|
|5|+@fairplay|
|6|+@networking|
|7|+@manifests|
|8|+@polyfill|
|9|+@text|
|10|+@ui|

per esempio in ads importo tutti i file che mi servono per la pubblicità e li posso modificare e aggiungere il mio file che mi serve o voglio sovrascirvere
build/build.py +@dds --force --mode release --name dds nella folder dds read me

---

goog.provide('shaka.cast.CastProxy'); export
goog.require('shaka.Player'); import
quando i moduli non era supportato

@privare vuole dire veramente che è privato e inaccessibile

---
