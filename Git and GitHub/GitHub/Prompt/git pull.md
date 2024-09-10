# git pull

`git pull` is another command we can use to retrieve changes from a remote repository. _Unlike fetch, pull actually updates our HEAD branch with whatever changes are retrieved from the remote_

"go and download data from Github AND immediately update my local repo with those changes"

## git pull = git fetch + git merge

- _git pull = git fetch + git merge_
  - git fetch
    - update the remote tracking branch with the latest changes from the remote repository
  - git merge
    - update my current branch with whatever changes are on the remote tracking branch

---

## Pulling

To pull, we specify the particular remote and branch we want to pull using `git pull <remote> <branch>`. Just like with git merge, it matters WHERE we run this command from. Whatever branch we run it from is where the changes will be merged into.

`git pull origin master` would fetch the latest information from the origin's master branch and merge those changes into our current branch.

```bash
git pull <remote> <branch>
```

[[git pull.png]]

---

### An even easier syntax!

If we run git pull without specifying a particular remote or branch to pull from, git assumes the following

- remote will default to origin
- branch will default to whatever tracking connection is configured for your current branch

```bash
git pull
```

[[git pull easier syntax.png]]

#### Note

this behavior can be configured, and tracking connections can be changed manually. Most people dont mess with that stuff

---
