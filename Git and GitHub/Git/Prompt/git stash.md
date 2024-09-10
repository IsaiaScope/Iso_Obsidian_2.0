# git stash

in git is possible to save changes in a stash, because is not possible to switch branch if same file modification could be cause of conflict to the branch you are switching to.
So you need to 'save' changes in a stash or just having changes that could cause problem while moving around branches

## Stashing

Git provides an easy way of stashing these uncommitted changes so that we can return to them later, without having to make unnecessary commits.

git stash is super useful command that helps you save changes that you are not yet ready to commit. You can stash changes and then come back to them later

Running `git stash` will take all uncommitted changes (staged and unstaged) and stash them, reverting the changes in your working copy.

- `git stash save` has the same result as follow

```bash
git stash
```

---

### Heads Up!

If you have untracked files (files that you have never checked in to Git), they will not be included in the stash.

Fortunately, you can use the `-u` option to tell git stash to include those untracked files.

```bash
git stash -u
```

---

## Re-apply lash stash charges

- Use `git stash pop` to _remove_ the most recently stashed changes in your stash and _re-apply them to your working copy_

```bash
git stash pop
```

---

## Stash Apply

You can use` git stash apply` to apply whatever is stashed away, _without removing_ it from the stash. This can be useful if you want to apply stashed changes to multiple branches.

git applies _most recent stash_ when you run `git stash apply`

```bash
git stash apply
```

---

## Stashing Multiple Times

You can add multiple stashes onto the stack of stashes. They will all be stashed in the order you added them.

- Viewing Stashes
  - run `git stash list` to view all stashes

```bash
git stash list
```

---

### Applying Specific Stashes

git assumes you want to apply the most recent stash when you run git stash apply, but you can also specify a particular stash like `git stash apply stash@{2}`

```bash
git stash apply stash@{2}
```

---

### Dropping Stashes

To delete a particular stash, you can use `git stash drop <stash-id>`

```bash
git stash drop stash@{2}
```

### Clearing The Stash

To clear out all stashes, run `git stash clear`

```bash
git stash clear
```

---
