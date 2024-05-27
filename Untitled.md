## Installing the plugin

[](https://github.com/DylanGiesberts/obsidian-table-checkboxes?tab=readme-ov-file#installing-the-plugin)


1. Navigate to the plugins store (Settings => Community plugins -> Browse)
2. Search for 'Smart Select Plugin'
3. Select the plugin and click Install

## How to use

[](https://github.com/DylanGiesberts/obsidian-table-checkboxes?tab=readme-ov-file#how-to-use)

- Simply enable the plugin and type a markdown checkbox inside a table. It will get converted to a HTML checkbox.
- In either live preview or view mode, (un)check the checkbox and the state will be reflected in your file.

## How it works

[](https://github.com/DylanGiesberts/obsidian-table-checkboxes?tab=readme-ov-file#how-it-works)

- Whenever a closing bracket `]` is typed to close a checkbox, it will be replaced by an HTML checkbox `<input type="checkbox" unchecked id="...">`.
- When the checkbox is clicked in the preview, the checkbox in the file is found by its ID.
- The `checked` state of the checkbox gets toggled.