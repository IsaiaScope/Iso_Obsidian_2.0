# git branch

- Use git branch to view your existing branches.
- The default branch in every git repo is master, though you can configure this.
- Look for the `*` which indicates the branch you are currently on.
- _Remember, branches are made based on the HEAD_

```bash
git branch
```

---

- Viewing More Info
  - Use the `-v` flag with git branch to view more information about each branch.

```bash
git branch -v
```

---

## Creating a new local branch

- Use `git branch <branch-name>` to make a new branch _locally_ based upon the current HEAD
- `git branch` _doesn't switch automatically_

```bash
git branch <branch-name>
```

---

## Remane a local branch

- Use the `-m` flag with `git branch` to remane the branch _locally_ based upon the current HEAD

```bash
git branch -m <branch-name>
```

---

## Delete a local branch

- Use the `-d` flag with git branch to delete a local branch.
- `-D` flag is useful when the following error happens
  - error: The branch `<branch-name>` is not fully merged. If you are sure you want to delete it, run `git branch -D <branch-name>`
  - in other words Git knows that you are gonna lose some changes forever if you delete that branch so asks you to force the deletion with `-D` flag. Because Git knows that you haven't merged all changes yet.

```bash
git branch -d <branch-name>
```

---
