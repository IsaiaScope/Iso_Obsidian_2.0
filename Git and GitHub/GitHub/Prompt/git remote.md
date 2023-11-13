# git remote

## Remote

Before we can push anything up to Github, we need to tell Git about our remote repository on Github. We need to setup a "destination" to push up to

In Git, we refer to these "destinations" as remotes. _Each remote is simply a URL where a hosted repository lives_.

### Viewing Remotes

To view any existing remotes for you repository, we can run `git remote` or `git remote -v` (verbose, for more info)
This just displays a list of remotes. If you haven't added any remotes yet, you won't see anything!

```bash
git remote
```

---

## [[Make a Repo]]

---

## Other commands

They are not commonly used, but there are commands to rename and delete remotes if needed.

```bash
git remote rename <old> <new>
```

```bash
git remote remove <name>
```

---
