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

## Amending Commits

- Suppose you just made a commit and then realized you forgot to include a file! Or, maybe you made a typo in the commit message that you want to correct.
- Rather than making a brand new separate commit, you can "redo" the previous commit using the `--amend` option
- 