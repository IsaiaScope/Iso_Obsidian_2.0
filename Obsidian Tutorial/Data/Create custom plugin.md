# Create custom plugin

## Creation Flow

[Build a plugin](https://docs.obsidian.md/Plugins/Getting+started/Build+a+plugin)
Use [Hot Reload](https://github.com/pjeby/hot-reload) for live development reload
[Official APIs](https://github.com/marcusolsson/obsidian-plugin-docs/blob/main/docs/user-interface/index.md)
[Icons](https://lucide.dev/icons/battery-charging)
[Submit your plugin](https://docs.obsidian.md/Plugins/Releasing/Submit+your+plugin)

### Note (How To Change Repo References)

After `git clone https://github.com/obsidianmd/obsidian-sample-plugin.git` change the repo `git remote set-url origin https://github.com/IsaiaScope/smart-html-select-plugin.git` and

The `git branch -M` and `git branch -m` commands are used to rename branches in Git, but they behave slightly differently:

- `git branch -m <newname>`: This command will rename the current branch to `<newname>`. If a branch with `<newname>` already exists, Git will throw an error.
- `git branch -M <newname>`: This command will also rename the current branch to `<newname>`. However, if a branch with `<newname>` already exists, it will be replaced without any warning.

So, the `-M` option is a more forceful version of `-m`, as it will overwrite an existing branch if one exists with the new name.

```
git branch -M dev
```

```
git branch --set-upstream-to origin/dev
```

if doesn't work use: `git push -u origin dev`

### Note (important set up)

- `repo` is the path to your GitHub repository. For example, if your GitHub repo is located at https://github.com/your-username/your-repo-name, the path is `your-username/your-repo-name`.
- Remember to add a comma after the closing brace, `}`, of the **previous entry**.
- A `LICENSE` (LICENCE.md) that determines how others are allowed to use the plugin and its source code. If you need help to [add a license](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/adding-a-license-to-a-repository) for your plugin, refer to [Choose a License](https://choosealicense.com/).
- `id` is unique to your plugin. Search `community-plugins.json` to confirm that there's no existing plugin with the same id. The `id` can't contain `obsidian` or `plugin`.

### Note (tag to release)

add a tag to release and trigger git hub action

```
git tag -a 1.0.1 -m "1.0.1"
```

```
git push origin 1.0.1
```

## smart-html-select-plugin

[smart-html-select-plugin](https://github.com/IsaiaScope/smart-html-select-plugin)

## Install Non Official Plugin

[YT Install Non Official Plugin](https://www.youtube.com/watch?v=ffGfVBLDI_0)

---
