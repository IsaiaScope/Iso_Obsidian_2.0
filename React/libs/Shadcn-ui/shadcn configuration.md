---
tags:
- shadcn_ui
---

# shadcn configuration

Run the `shadcn-ui` init command to setup your project:

> [shadcn + nextjs](https://ui.shadcn.com/docs/installation/next)

```bash
npx shadcn-ui@latest init
```

## folder structure

.
├── app
│ ├── layout.tsx
│ └── page.tsx
├── _components_
│ ├── _shadcn-ui_
│ │ ├── alert-dialog.tsx
│ │ ├── button.tsx
│ │ ├── dropdown-menu.tsx
│ │ └── ...
│ ├── main-nav.tsx
│ ├── page-header.tsx
│ └── ...
├── _libraries_
│ ├── _tailwindcss_
│ │└── _twMerge-clsx-utility-class.ts_
│ └── ...
├── _styles_
│ └── _globals.css_
├── next.config.js
├── package.json
├── _components.json_
├── postcss.config.js
├── tailwind.config.js
└── tsconfig.json

## components.json

[components-json](https://ui.shadcn.com/docs/components-json)

- `aliases.tils` is the import alias for your utility functions.
- The CLI will use the `aliases.ui` value to determine where to place your `ui` components. Use this config if you want to customize the installation directory for your `ui` components.
- `aliases.components` is the import alias for your components.

```json
{
	"$schema": "https://ui.shadcn.com/schema.json",
	"style": "new-york",
	"rsc": false,
	"tsx": true,
	"tailwind": {
		"config": "tailwind.config.ts",
		"css": "@/styles/globals.css",
		"baseColor": "gray" | "neutral" | "slate" | "stone" | "zinc",
		"cssVariables": false,
		"prefix": ""
	},
	"aliases": {
		"components": "@/components",
		"utils": "@/libraries/tailwindcss/twMerge-clsx-utility-class",
		"ui": "@/components/shadcn-ui"
	}
}
```

## That's it

You can now start adding components to your project.

```
npx shadcn-ui@latest add button
```

---
