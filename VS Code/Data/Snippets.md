---
tags:
  - VS_Code
---

# Snippets

[DOC Snippet](https://code.visualstudio.com/docs/editor/userdefinedsnippets)

## Little Example

`1` and `2` select automatically the variable and permit `tab` navigation

```json
{
	"arrow function": {
		// "scope": "typescript,typescriptreact,javascript,javascriptreact",
		"prefix": ["code an arrow function"],
		"body": [
			"const ${1:arrow$RANDOM} = (${2:params}) => {",
			"\tconsole.log(`[Iso_DEBUG]`, ${2:params})",
			"};"
		],
		"description": "create empty arrow function"
	}
}
```

---
