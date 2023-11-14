# Remote Branches

## Remote Tracking Branches

"At the time you last communicated with this remote repository, here is where x branch was pointing

They follow this pattern `<remote>/<branch>`.

- _origin/master_ references the state of the master branch on the remote repo named origin.
- _upstream/logoRedesign_ references the state of the logoRedesign branch on the remote named upstream (a common remote name)

[[Remote Branches.png]]

---

## Remote Branches

Run `git branch -r` to view the remote branches our local repository knows about.

```bash
git branch -r
```

### You can checkout these remote branch pointers

- Note: switching to 'origin/master'. You are in 'detached HEAD' state. You can look around, make experimental changes and commit them, and you can discard any commits you make in this blah blah blah blah
- Detached HEAD! Don't panic. It's fine

```bash
git checkout origin/master
```

---

## Checkout remote branch pointers and get a local branch with same name setting up the stream

- Run `git switch <remote-branch-name>` to create a new local branch from the remote branch of the same name.
- `git switch puppies` makes me a local puppies branch AND sets it up to track the remote branch origin/puppies.

```bash
git switch puppies
```

---

## Feature Branch Naming

There are many different approaches for naming feature branches. Often you'll see branch names that include slashes like _bug/fix-scroll_ or _feature/login-form_ or _feat/button/enable-pointer-events_
Specific teams and projects usually have their own branch naming conventions. To keep these slides simple and concise, I'm just going to ignore those best- practices for now.

---

