# git fetch

## Fetching

Fetching allows us to download changes from a remote repository, _BUT those changes will not be automatically integrated into our working files_.

- It lets you see what others have been working on, without having to merge those changes into your local repo.
- Think of it as "please go and get the latest information from Github, but don't screw up my working directory."

The `git fetch <remote>` command fetches branches and history from a specific remote repository. It only updates remote tracking branches.
`git fetch origin` would fetch all changes from the origin remote repository.

```bash
git fetch <remote>
```

_If not specified_, `<remote>` _defaults to origin_

[[git fetch.png]]

---

## Fetch a specific branch

We can also fetch a specific branch from a remote using `git fetch <remote> <branch>`

- For example, git fetch origin master would retrieve the latest information from the master branch on the origin remote repository.

```bash
git fetch <remote> <branch>
```

---
