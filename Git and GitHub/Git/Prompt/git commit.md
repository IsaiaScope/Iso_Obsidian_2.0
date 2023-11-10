# git commit

- _Committing_
  - Making a commit is similar to making a save in a video game. We're taking a snapshot of a git repository in time. When saving a file, we are saving the state of a single file. With Git, we can save the state of multiple files and folders together
  - We use the git commit command to actually _commit changes from the staging area_.
  - When making a commit, _we need to provide a commit message_ that summarizes the changes and work snapshotted in the commit
  - [[Commit flow.png]]

---

- The `-m` flag allows us to pass in an inline commit message, rather than launching a text editor.

```bash
git commit -m "my message"
```

---

## Change default Git text editor

- `git commit` It also opens up a text editor (VIM is default) and prompts you for a commit message.

```bash
git commit
```

- To change default Git text editor
  - [A-Git-Commands-Setup-and-Config](https://git-scm.com/book/en/v2/Appendix-C%3A-Git-Commands-Setup-and-Config)
- Following snippet sets VS Code as default editor per Git
  - `--wait` means u need to close the file to ensure the commit, just saving isn't enough
  - consent to make much longer and multi line commit messages

```bash
git config --global core.editor "code --wait"
```

---

## Atomic Commits

- When possible, a _commit should encompass a single feature_, change, or fix. In other words, try to keep each commit focused on a single thing.
- This makes it much easier to undo or rollback changes later on. It also makes your code or project easier to review.
- Make commits atomic (group similar changes together, don't commit a million things at once)

---

## Amending Commits

- Suppose you just made a commit and then realized you _forgot to include a file! Or, maybe you made a typo in the commit message that you want to correct_.
- Rather than making a brand new separate commit, you can "redo" the previous commit using the `--amend` option
- `--amend` _woks just for one commit ago_
- after doing a normal commit

```bash
 git commit -m 'some commit'
```

- is possible to work more in your workspace and make some changes to files or create new ones, than stage all with `git add`

```bash
 git add .
```

- `git commit --amend` is gonna open text editor where is possible to change commit message and file changes are already registered, so if you don't need to change commit message just close the text editor, and the commit is gonna happen immediately after. _This commit doesn't create a new one but overwrite the last one keeping old changes and new ones_

```bash
git commit --amend
```

---
