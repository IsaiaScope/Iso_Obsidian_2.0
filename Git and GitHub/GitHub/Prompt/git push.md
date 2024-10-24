# git push

if we have a remote set up, let's push some work up to Github! To do this, we need to use the `git push` command

We need to specify the remote we want to push up to AND the specific local branch we want to push up to that remote

```bash
git push <remote> <branch>
```

---

- git push origin master tells git to push up the master branch to our origin remote.

```bash
git push origin master
```

[[git push.png]]

---

## Connecting our local branch to a branch on Github

The `-u` option allows us to set the upstream of the branch we're pushing
You can think of this as a link connecting our local branch to a branch on Github.

- [[git remote#Remote | Remote]]
  - get `<remote>` from `git remote`

```bash
git push -u <remote> <branch>
```

- Running `git push -u` origin master sets the upstream of the local master branch so that it tracks the master branch on the origin repo.

```bash
git push -u origin master
```

### What this means...

- Once we've set the upstream for a branch, we can use the `git push` shorthand which will push our current branch to the upstream.

- flag `-u` stand for `--set-upstream`

[[git push -u.png]]

- is possible to set upstream between branches not called the same
  - _best practise is to not use_

```bash
git push -u origin <local-branch>:<remote-branch>
```

---

## Push In Detail

- While we often want to push a local branch up to a remote branch of the _same name, we don't have to!_
- local brach and remote branch could have different name

```bash
git push <remote> <local-branch>:<remote-branch>
```

- To push our local pancake branch up to a remote branch called waffle we could do: git push origin pancake:waffle

```bash
git push origin pancake waffle
```

---

## Delete Remote Branch

- [[git remote#Remote | Remote]]
  - get `<remote>` from `git remote`
  - default remote is `origin`

```bash
git push <remote> --delete <remote-branch-name>
```

---

## Force Push

- `-f` or `--force`: This flag is used to force the push operation, meaning that it will overwrite changes on the remote branch with the changes from your local branch, even if they conflict. It's a powerful command and should be used with caution, especially in a collaborative environment.

```bash
git push -f origin <remote-branch-name>
```

The command `git push -f origin dev` is a Git command used to forcefully push changes from your local Git repository to a remote repository, specifically to the `dev` branch on the remote repository named `origin`.

```bash
git push -f origin dev
```

---
