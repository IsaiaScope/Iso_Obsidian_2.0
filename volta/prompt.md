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
5. Check version
```console
volta list
```
6. Help pannel
```console
The JavaScript Launcher ⚡
    To install a tool in your toolchain, use `volta install`.
    To pin your project's runtime or package manager, use `volta pin`.
    
USAGE:
	 volta [FLAGS] [SUBCOMMAND]
	 
FLAGS:
	--verbose    
			Enables verbose diagnostics
	--quiet      
			Prevents unnecessary output
	-v, --version    
			Prints the current version of Volta
	-h, --help       
			Prints help information
			
SUBCOMMANDS:
    fetch          Fetches a tool to the local machine
    install        Installs a tool in your toolchain
    uninstall      Uninstalls a tool from your toolchain
    pin            Pins your project's runtime or package manager
    list           Displays the current toolchain
    completions    Generates Volta completions
    which          Locates the actual binary that will be called by Volta
    setup          Enables Volta for the current user / shell
    help           Prints this message or the help of the given subcommand(s)
```
---
