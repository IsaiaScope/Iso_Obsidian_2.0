---
tags:
  - Volta
---

# Set Default Globally

Move to root path and use `volta install` to select a default version of a tool.
The `volta install` command will set your default version of a tool. It will also fetch that tool if it isn’t already cached locally. It has the following syntax:

```bash
volta install <package-name>@<version>
```

```bash
volta install npm@9.7.2
```

---

> Use `volta install` to select also a already installed version, to see all versions installed do the following:

```
volta list <package-name>
```

---
