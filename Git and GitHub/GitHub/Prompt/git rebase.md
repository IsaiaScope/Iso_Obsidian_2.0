# git rebase

## Rebasing

- [understanding-git-rebase](https://community.appsmith.com/content/guide/understanding-git-rebase?ref=dailydev)

There are two main ways to use the `git rebase` command:

- as an alternative to merging
- as a cleanup tool

## Rebasing as Alternative to merging

![[git rebase starting point.png]]

Instead of Merging, we can instead rebase the feature branch onto the master branch. This moves the entire feature branch so that it BEGINS at the tip of the master branch. All of the work is still there, but we have re-written history.

_Instead of using a merge commit, rebasing rewrites history by creating new commits for each of the original feature branch commits_

```bash
git switch feature
```

```bash
git rebase master
```
 
![[git rebase.png]]

---

### Why Rebase?

We get a much cleaner project history. No unnecessary merge commits! We end up with a linear project history.

### WARNING!

Never rebase commits that have been shared with others. If you have already pushed commits up to GitHub...DO NOT rebase them unless you are positive no one on the team is using those commits.

You do not want to rewrite any git history that other people already have. It's a pain to reconcile the alternate histories!

---

## Rebasing as Cleanup Tool

### Rewriting History

Sometimes we want to _rewrite, delete, rename, or even reorder commits_ (before sharing them)
We can do this using git rebase!

---

### Interactive Rebase

Running `git rebase` with the `-i` option will enter the interactive mode, which allows us to edit commits, add files, drop commits, etc. Note that we need to specify how far back we want to rewrite commits.

Also, notice that we are not rebasing onto another branch. Instead, we are rebasing a series of commits onto the HEAD they currently are based on.

```bash
git rebase -i HEAD~4
```

![[git rebase cleaning tool.png]]

---
