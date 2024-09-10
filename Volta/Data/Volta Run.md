---
tags:
  - Volta
---

# Volta Run

The `volta run` command will run the command that you give it, using versions of tools that are specified at the command line.

Any tool that doesn’t have a version specified directly will have its version determined by Volta’s usual context detection, using the pinned versions in a project or the default versions.

```
volta run npm i
```

---
