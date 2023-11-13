# git revert

## Git Reset vs Git Revert

Yet another similar sounding and confusing command that has to do with undoing changes

`git revert` is similar to `git reset` in that they both "undo" changes, but they accomplish it in different ways

`git reset` actually moves the branch pointer backwards, eliminating commits

`git revert` instead _creates a brand new commit which reverses/undos the changes from a commit_. Because _it results in a new commit, you will be prompted to enter a commit message_

- [[HASH]]

```bash
git revert <commit-hash>
```

[[git reset Flow.png]]

[[git revert Flow.png]]
[[git revert commits console.png]]

### Which One Should I Use?

If you want to reverse some commits that other people already have on their machines, you should use _revert_

If you want to reverse commits that you haven't shared with others, use _reset_ and no one will ever know!

I use `git revert` to reverse the same commits as before, by ADDING a new commit to the chain
My team can merge in the new "undo" commit without issue. I didn't alter history.

#### When to Use `git reset` and `git revert`

You should use `git reset` when working on a local repository with changes yet to be pushed remotely. This is because running this command after pulling changes from the remote repo will alter the commit history of the project, leading to merge conflicts for everyone working on the project. `git reset` will undo changes up to the state of the specified commit ID.

`git reset` is a good option when you realize that the changes being made to a particular local branch should be somewhere else. You can reset and move to the desired branch without losing your file changes.

`git revert` is a good option for reverting changes pushed to a remote repository. Since this command creates a new commit, you can safely get rid of your mistakes without rearranging the commit history for everyone else. This command undoes the effects of a bad or incorrect commit. It creates a new head without the issues of the bad commit but doesn't revoke any previous work. However, this version contains history from the bad commit. This inclusion is by design: Git wants to track history accurately, and to delete current heads would create massive gaps in the system history.

---
