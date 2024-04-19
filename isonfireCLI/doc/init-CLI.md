# init-CLI

## [TSConfig bases](https://github.com/tsconfig/bases)

Recomanded TSConfig tuned to a particular runtime environment

- For a CLI: [node-lts](https://github.com/tsconfig/bases/blob/main/bases/node-lts.json)

## [Vite global scope definition](https://www.reddit.com/r/sveltejs/comments/14o51w3/comment/jqc8jy8/?share_id=D_K34qhC4AUaDOgdgV-Nr)

In `vite.config.ts` we define `PKG_VERSION` and `PKG_NAME`

```ts
import { resolve } from 'path';
import { defineConfig } from 'vite';
import { nodeExternals } from 'rollup-plugin-node-externals';
import pkg from './package.json' assert { type: 'json' };

export default defineConfig({
  build: {
    lib: {
      entry: resolve(__dirname, 'src/scripts/index.ts'),
      name: 'Bin',
      fileName: 'bin',
      formats: ['es'],
    },
  },
  plugins: [nodeExternals()],
  define: {
    PKG_VERSION: `"${pkg.version}"`,
    PKG_NAME: `"${pkg.name}"`,
  },
});
```

For type safe create `globals.d.ts`

```ts
export declare global {
  declare const PKG_VERSION: string;
  declare const PKG_NAME: string;
}
```

Usage

```ts
import { Command } from 'commander';
import { copy } from './commands/copy.js';
import { see } from './commands/see.js';
import { add } from './commands/add.js';

export const program = new Command();

program
  .version(PKG_VERSION, '-v, --version', 'check CLI version')
  .name(PKG_NAME)
  .description(
    'Copy from GitHub repository https://github.com/IsaiaScope/isonfireCLI'
  );

program.addCommand(copy).addCommand(see).addCommand(add);

program.parse(process.argv);
```

## [GitHub Action for npm publishing](https://github.com/devopshint/publish_to_npm_using_github_actions/blob/main/.github/workflows/publish.yml)

`NODE_AUTH_TOKEN` must be added in _GitHub Repo - Settings - Secrets and Variables - Action - Repository secrets_, the value can be obtain from npm profile token setting

```yml
name: Publish to NPM
on:
  push:
    branches:
      - '**'
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Setup pnpm
        uses: pnpm/action-setup@v2
        with:
          version: 8
      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: 20.x
          registry-url: 'https://registry.npmjs.org'
          cache: 'pnpm'

      - name: Install dependencies
        run: pnpm install
      - name: Build
        run: pnpm run build

      - name: Publish to npm
        run: pnpm release
        env:
          NODE_AUTH_TOKEN: ${{secrets.NODE_AUTH_TOKEN}}
```

## [reference-package-version-inside](https://stackoverflow.com/questions/48609931/how-can-i-reference-package-version-in-npm-script)

### Theory

`%npm_package_version%` evaluate the package version

### Usage

NOTE: `--no-git-tag-version` permit to remove auto commit and tag

```json
  "scripts": {
    "show-version": "echo %npm_package_version%",
    "commit-all": "git add . && git commit -m \"version: %npm_package_version%\" && git push",
    "push": "npm version patch --no-git-tag-version && pnpm commit-all"
  },
```

---
