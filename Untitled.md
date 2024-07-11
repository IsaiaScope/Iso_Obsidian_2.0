tsx flags supported
https://nodejs.org/docs/latest-v20.x/api/cli.html

---

pnpm to execute package.json scripts inside the workspace
https://pnpm.io/filtering

---

be hono
https://hono.dev/

---

tw
https://tailwindcss.com/docs/guides/vite

```shell
pnpm install -D prettier prettier-plugin-tailwindcss tailwindcss postcss autoprefixer tailwind-merge clsx
```
```
npx tailwindcss init -p
```

- [ ] after `npx tailwindcss init -p` than follow https://tailwindcss.com/docs/guides/vite
- [ ] create prettier.config.js in the root of all project

```js
module.exports = {
	plugins: ["prettier-plugin-tailwindcss"],
};
```

- [ ] create `twMerge-clsx-utility-class.ts`

```ts
import { type ClassValue, clsx } from 'clsx';
import { twMerge } from 'tailwind-merge';
 
export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```

- [ ] now u can style :)