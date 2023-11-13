

#### Migrate Repository to Another Url

- this change also reference in the project so next pull/push actions are gonna be on new Url

```console
git clone --mirror <URL OLD>
cd <directory where your OLD repo was cloned>
git remote set-url origin <NEW URL>
git push --mirror origin
```

---

# Push new branch to origin or delete

_first push_ to create the branch in the GitHub repo

- remember to do the commit first

```bash
git push --set-upstream origin <myNewBranch>
```

---

2. delete _remote_ branch

```console
git push origin --delete <remoteBranchName>
```

---

#### Check Url Repo or Directory where local repo-clone is

1. check Url Repo

```console
git remote show origin
```

2. check directory where local repo-clone is

```console
git rev-parse --show-toplevel
```

---

#### How to update my old working Git branch from another branch

```console
git checkout <old_branch_to_update>
git merge <branch_where_take_data_from>
```

---

#### Check Commits & Create a New branch from a specific commit

1. Create a New branch from a specific commit and checkout it

```console
git checkout -b branch_name <commit-hash or HEAD~3>
```

---

#### Cherry Pick

1. add a specific commit to a branch
2. get hash by lunching git log in original branch where u want to take commit hash code
3. positioning yourself on the branch to update with the hash code of the commit and lunch cherry-pick
4. cherry-pick make automatic commit; to avoid that put -n flag

```console
git cherry-pick <commit-hash>
```

---

#### Change commit message not pushed

```
git commit --amend -m "New commit message"
```

---
