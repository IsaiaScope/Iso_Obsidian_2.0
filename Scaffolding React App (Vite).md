# Backend

tsx flags supported
https://nodejs.org/docs/latest-v20.x/api/cli.html

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

- [ ] in `vite.config.ts`, forward any request that start with `/api` to port 3000 actually our BE

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

### prettier-plugin-tailwindcss Installation
#### First Option
- [ ] create `prettier.config.js` in the frontend folder of all project (if doesn't at the start restart vs config)

```js
export default {
	plugins: ["prettier-plugin-tailwindcss"],
};
```
#### Second Option Monorepository Approach




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
