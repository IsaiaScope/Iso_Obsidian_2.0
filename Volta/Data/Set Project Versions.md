---
tags:
  - Volta
---

# Set Project Versions

The `volta pin` command will update a project’s `package.json` file to use the selected version of a tool.

---

Project Root

```bash
volta pin <package-name>@<version>
```

---

Result in package.json

```json
"volta": {
	"node": "16.13.1",
	"npm": "8.19.3"
}
```

---
