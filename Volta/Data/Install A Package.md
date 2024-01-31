---
tags:
  - Volta
---

# Install A Package

U can install other packages or using normal cmd _npm i [name]_

```
npm i -g <package_name>
```

```bash
volta install <package_name>
```

With Volta, installing a command-line tool globally with your package manager also adds it to your [[Volta/Data/Theory#Toolchain|Toolchain]].

When you install a package to your toolchain, Volta takes your current default Node version and *pins* the tool to that engine (see [Package Binaries](https://docs.volta.sh/advanced/packages#pinned-node-version) for more information). Volta won’t change the tool’s pinned engine unless you update the tool, no matter what. This way, you can be confident that your installed tools don’t change behind your back.

---
