1. anaging your toolchain
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

---
2. Using project tools

The `node` and package manager executables aren’t the only smart tools in your toolchain: the package binaries in your toolchain are also aware of your current directory, and respect the configuration of the project you’re in.

For example, installing the Typescript package will add the compiler executable—`tsc`— to your toolchain:

```
npm install --global typescript
```

Depending on the project you’re in, this executable will switch to the project’s chosen version of TypeScript:

```
cd /path/to/project-using-typescript-3.9.4
tsc --version # 3.9.4

cd /path/to/project-using-typescript-4.1.5
tsc --version # 4.1.5
```

- Safety _and_ convenience

Because Volta’s toolchain always keeps track of where you are, it makes sure the tools you use always respect the settings of the project you’re working on. This means you don’t have to worry about changing the state of your installed software when switching between projects.

What’s more, Volta covers its tracks whenever it runs a tool, making sure your npm or Yarn scripts never see what’s in your toolchain.

These two features combined mean that Volta **solves the problem of global packages**. In other words, Volta gives you the _convenience_ of global package installations, but without the _danger_.

For example, you can safely install TypeScript with `npm i -g typescript`—and enjoy the convenience of being able to call `tsc` directly from your console—without worrying that your project’s package scripts might accidentally depend on the global state of your machine.

---