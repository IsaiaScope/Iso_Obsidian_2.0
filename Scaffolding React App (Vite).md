# Backend

tsx flags supported
https://nodejs.org/docs/latest-v20.x/api/cli.html

---

pnpm to execute package.json scripts inside the workspace
https://pnpm.io/filtering

---

## Hono for server building

[Hono[(https://hono.dev/docs/)

```
pnpm create hono@latest
```

---

# Frontend

## Style

### Tailwind

```bash
pnpm install -D prettier prettier-plugin-tailwindcss tailwindcss postcss autoprefixer tailwind-merge clsx
```

```
npx tailwindcss init -p
```

- [ ] after `npx tailwindcss init -p` than follow [Install Tailwind CSS with Vite](https://tailwindcss.com/docs/guides/vite)
- [ ] create `prettier.config.js` in the root of all project

```js
module.exports = {
	plugins: ["prettier-plugin-tailwindcss"],
};
```

- [ ] create `twMerge-clsx-utility-class.ts` under `src/utility/style` path

```ts
import { type ClassValue, clsx } from 'clsx';
import { twMerge } from 'tailwind-merge';

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```

- [ ] now u can style :)
- [ ] _NOTE_ in `tailwind.config.ts` you cannot use `tsconfig.json` path aliases

---

## UI

### Shadcn

- Init setup for [[shadcn]]
- Setup [[React/Libs/Shadcn-ui/style|style]] for shadcn

Step to configure with [`vite@5.3.1`](https://ui.shadcn.com/docs/installation/vite)

- [ ] `npx shadcn-ui@latest init` and answer all question for `components.json` creation
- [ ] add to `tsconfig.json` because putting this alias into `tsconfig.app.json` doesn't work in corrently in `components.json`

```json
  "compilerOptions": {
    "paths": {
      // shadcn read just `tsconfig.json` so we need to add it here for components.json
      "@shadcn": ["./src/components/shadcn"]
    }
  }
```

- [ ] compile `components.json` as follow:

```json
{
	"aliases": {
		"components": "@shadcn",
		"utils": "@cn",
		"ui": "@shadcn"
	}
}
```

- [ ] in `tsconfig.app.json` for `utils` and `components` import for downloaded shadcn components

```json
	"compilerOptions": {
	    "paths": {
	      "@cn": ["./src/utility/style/twMerge-clsx-utility-class.ts"],
	      // this alias is for the shared components import correct into components/shadcn folder
	      "@shadcn/*": ["./src/components/shadcn/*"],
	 }
```

- [ ] in `vite.config.ts` must resolve the same path describe into `tsconfig.app.json` and `tsconfig.json` and all other ones
- [ ] _NOTE_ a useful escape could be `vite-tsconfig-paths` and npm pkg that import automatically paths from all `tsconfig.json`

```ts
  resolve: {
    alias: [
      {
        find: "@shadcn",
        replacement: path.resolve(__dirname, "./src/components/shadcn"),
      },
      {
        find: "@cn",
        replacement: path.resolve(
          __dirname,
          "./src/utility/style/twMerge-clsx-utility-class.ts",
        ),
      },
    ],
  },
```

---
