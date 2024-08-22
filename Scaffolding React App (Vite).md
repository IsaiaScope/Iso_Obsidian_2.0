# Backend

tsx flags supported
https://nodejs.org/docs/latest-v20.x/api/cli.html

to get `.js` in `tsc` build must have

```
"target": "ESNext",
"module": "NodeNext",
"moduleResolution": "NodeNext",
```

- _NOTE_ `"moduleResolution": "Bundler",` omit that and cause and error when running build
- with `moduleResolution": "NodeNext` imports must have `.js` even the file is a `ts` works fine

---

turbo

https://turbo.build/repo/docs/getting-started/installation

---

pnpm to execute package.json scripts inside the workspace
https://pnpm.io/filtering

---

## Hono for server building

[Hono](https://hono.dev/docs/)

```
pnpm create hono@latest
```

## Setup a proxy (Avoid CORS in develop)

- [ ] in `vite.config.ts`, forward any request that start with `/api` to port 3000 actually our BE. In frontend project

```json
  server: {
    proxy: {
     "/api": {
	     target: "http://127.0.0.1:3000",
	     changeOrigin: true,
      },
    }
  },
```

- [ ] _NOTE_ if `target: http://localhost:3000` doesn't work mean your server is not configured to listen on IPv6, you can force Vite to use the IPv4 address `127.0.0.1` instead.
- [ ] _NOTE_ example: `FE: fetchUrl = "/api/events"`; `BE: "/api/events" must be a valid route`

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
- [ ] create `prettier.config.js` in the frontend folder of all project (if doesn't at the start restart vs config)

```js
export default {
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
- [ ] _NOTE_ a useful escape could be `vite-tsconfig-paths` a npm pkg that import automatically paths from all `tsconfig.json`

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

## i18next

- `import { useTranslation } from 'react-i18next';`
- ` const { t, i18n: { changeLanguage, language } } = useTranslation(["common", "games"]);`

### Tutorial for react-i18next

- [react-i18next](https://i18nexus.com/tutorials/nextjs/react-i18next)

### Create Translations

- [i18nexus](https://app.i18nexus.com) (Site for translations and where pull json form)
  Scripts to pull json and update type safe into code
- [YT: type safe video guide](https://youtu.be/GLIas4DH3Ww?si=HNX_qCgYpr_gm38d&t=79 "https://youtu.be/GLIas4DH3Ww?si=HNX_qCgYpr_gm38d&t=79")

```shell
// package.json
"create:translations:json": "i18nexus pull -k hv1KQnCWBHgIGsCIJTx8gg -p ./public/locales",
"create:translations:interface": "i18next-resources-for-ts interface -i ./public/locales/en-GB -o ./src/@types/i18next/resources.d.ts",
"update:locales": "pnpm create:translations:json && pnpm create:translations:interface"
```

- _NOTE_ hv1KQnCWBHgIGsCIJTx8gg apiKey comes from i18nexus
- _NOTE_ to achive type safety types must be put in @types folder
- _NOTE_ `import "@/utility/i18next/i18next";`
- _NOTE_ `Suspense` the main app

---

## Tanstack Router
