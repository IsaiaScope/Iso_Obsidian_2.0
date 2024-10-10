# reference-package-version-inside

## Theory

`%npm_package_version%` evaluate the package version

## Resources

[stackoverflow](https://stackoverflow.com/questions/48609931/how-can-i-reference-package-version-in-npm-script)

## Usage

```json
  "scripts": {
    "show-version": "echo %npm_package_version%",
    "commit-all": "git add . && git commit -m \"version: %npm_package_version%\" && git push",
    "push": "npm version patch --no-git-tag-version && pnpm commit-all"
  },
```

---
