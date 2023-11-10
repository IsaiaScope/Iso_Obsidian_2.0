# git init

- Use git init to create a new git repository. Before we can do anything git-related, we must initialize a repo first!
- This is something you do once per project. Initialize the repo in the top-level folder containing your projec
- _Before running git init, use git status to verify that you are not currently inside of a repo_

```bash
git init
```

- This command creates an empty Git repository - basically a `.git` directory with subdirectories for `objects`, `refs/heads`, `refs/tags`, and template files. An initial branch without any commits will be created
- to see `.git` directory run the following snippet because is hidden

```
ls -a
```

---

- If you do end up making a repo inside of a repo, you can delete it and try again!
- To delete a repo, locate the associated .git directory and delete it.
- to remove `.git` directory and all history in that folder

```
rm -rf .git
```

---

- `--initial-branch=<branch-name>` can be added to change initial branch name

```bash
git init --initial-branch=test
```
