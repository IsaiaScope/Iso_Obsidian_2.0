1. *need admin*, remember to set node versione or/and npm versione in package.json by using following cmd
```js
"volta": {
	"node": "16.13.1",
	"npm": "8.19.3"
}
```
```console
volta pin node@16.13.1
```

2. now u can install other packages or using normal cmd ***npm i [name]***
```console
volta install <name>
```
3. With Volta, installing a command-line tool globally with your package manager also adds it to your toolchain. 
```
npm i -g <package_name>
```
When you install a package to your toolchain, Volta takes your current default Node version and _pins_ the tool to that engine (see [Package Binaries](https://docs.volta.sh/advanced/packages#pinned-node-version) for more information). Volta won’t change the tool’s pinned engine unless you update the tool, no matter what. This way, you can be confident that your installed tools don’t change behind your back.
4. Check version
```console
node --version
yarn --version
```
---
5. anaging your toolchain
You control the tools managed by your Volta toolchain with two commands: `volta install` and `volta uninstall`.

***Installing Node engines***

To install a tool to your toolchain, you set the _default version_ of that tool. Volta will always use this default, unless you’re working within a project directory that has configured Volta to use a different version. When you choose a default version, Volta will also download that version to the local cache.

For example, you can select an exact version of `node` to be your default version:

```
volta install node@14.15.5
```

You don’t need to specify a precise version, in which case Volta will choose a suitable version to match your request:

```
volta install node@14
```

You can also specify `latest`or even leave off the version entirely, and Volta will choose the latest LTS release:

```
volta install node
```

Once you’ve run one of these commands, the `node` executable provided by Volta in your `PATH` environment (or `Path` in Windows) will, by default, automatically run your chosen version of Node.

Similarly, you can choose versions of the npm and Yarn package managers with `volta install npm` and `volta install yarn`, respectively. These tools will run using the default version of Node you selected