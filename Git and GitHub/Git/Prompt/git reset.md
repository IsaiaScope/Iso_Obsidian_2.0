# git reset

Suppose you've just made a couple of commits on the master branch, but you actually meant to make them on a separate branch instead._ To undo those commits, you can use_ `git reset`

- `git reset <commit-hash>` will reset the repo back to a specific commit. _The commits are gone and lost from the current branch and all changes from discard commits will apply in our working tree_
  - [[HASH]]

```bash
git reset <commit-hash>
```

[[Git and GitHub/Git/Files/git reset.png | git reset.png]]

---

## Reset `--hard`

If you want to undo both the commits AND the actual changes in your files, you can use the `--hard` option

- for example, `git reset --hard HEAD~1` _will delete the last commit and associated changes_

```bash
git reset --hard <commit>
```

[[git reset --hard.png]]

---

## [[git revert#Git Reset vs Git Revert | Git Reset vs Git Revert]]

---

## Reset Origin to A specific Commit

```bash
git reset --hard <commit>
```

> The `--force` flag overrides this behavior and makes the remote repository’s branch match your local one, deleting any upstream changes that may have occurred since you last pulled. The only time you should ever need to force push is when you realize that the commits you just shared were not quite right and you fixed them with a `git commit --amend` or an interactive rebase. However, you must be absolutely certain that none of your teammates have pulled those commits before using the `--force` option.

```bash
git push -f origin feat/anonymous-login
```

---
