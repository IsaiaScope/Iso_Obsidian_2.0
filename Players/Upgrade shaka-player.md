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

  - dds (folder with custom business logic to add)
  - build/types/dds (for saying witch js files include and exclude from the build)
    - /ui/language_utils.js (the original from shaka repo is removed)
    - /dds/language_utils.js (our custom one is added)

- [ ] In shaka-player repo project copy `types/dds` and `build/dds`
- [ ] Than update `dds/language_utils.js` with `ui/language_utils.js` but make sure to override the part of language mapping; we need to replace `mozilla.LanguageMapping` with `shaka.ui.LanguageMapping` and use that instance instead the mozilla one

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

- [ ] Now we are ready to build the code, place yourself on shaka-player repo project and launch

```bash
build/build.py +@dds --force --mode release --name dds
```

- [ ] A new file will be generate _dist/shaka-player.dds.js_ thats containt our _shaka build_ (note: those files are hide by git ignore so don't show for commit)
- [ ] Copy that wherever you want

### Note

build/types/complete holds the build configuration and make possible to customize the build.

```
+@ads
+@cast
+@fairplay
+@networking
+@manifests
+@polyfill
+@text
+@ui
```

for example `ads` imports all file needed for advertising with the possibility to add or remove files with plus or minus sign

#### Our custom command

create a @complete build because build/types/dds holds build/types/complete and rename the build with dds inside: `shaka-player.dds.js`

```
build/build.py +@dds --force --mode release --name dds
```

---
