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

![[Pasted image 20231113154449.png]]
![[Pasted image 20231113154515.png]]

### Which One Should I Use?

If you want to reverse some commits that other people already have on their machines, you should use revert

If you want to reverse commits that you haven't shared with others, use reset and no one will ever know!
