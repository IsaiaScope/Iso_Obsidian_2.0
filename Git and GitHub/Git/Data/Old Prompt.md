#### Change default branch

- You need to go to the GitHub page for your forked repository, and click on the “Settings” button.
- Click on the "Branches" tab on the left hand side. There’s a “Default branch” drop-down list near the top of the screen. Where u can change the default branch

---

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

#### Revert a branch to a specific commit

- When to Use `git reset` and `git revert`
  You should use `git reset` when working on a local repository with changes yet to be pushed remotely. This is because running this command after pulling changes from the remote repo will alter the commit history of the project, leading to merge conflicts for everyone working on the project. `git reset` will undo changes up to the state of the specified commit ID.

`git reset` is a good option when you realize that the changes being made to a particular local branch should be somewhere else. You can reset and move to the desired branch without losing your file changes.

`git revert` is a good option for reverting changes pushed to a remote repository. Since this command creates a new commit, you can safely get rid of your mistakes without rearranging the commit history for everyone else. This command undoes the effects of a bad or incorrect commit. It creates a new head without the issues of the bad commit but doesn't revoke any previous work. However, this version contains history from the bad commit. This inclusion is by design: Git wants to track history accurately, and to delete current heads would create massive gaps in the system history.

1. get hash by lunching git log cmd branch where u want to take commit hash code

```console
git revert <commit-hash>
```

Alternatively, there is a shorthand method to roll back the commit: To revert commits without knowing the necessary commit ID, admins can use the command below to revert code versions relative to where the current head is. In the example, **~1** refers to the number of commits backward to which the code tree will revert. Figure 1 illustrates the results for adding several commits and then reverting back one version.

```
git reset head~1
```

**Merge** => How to Revert the Last Merge Commit in Git

```
git revert -m 1 commit_hash
```

---

#### Revert a specific file

- You can view the list of commits that modified `package-lock.json` using :
  ```
  git log package-lock.json
  ```
- You can set `package-lock.json` back to its version in commit `[ID]` using :
- ID = **HASH**
  ```
  git checkout [ID] -- package-lock.json
  ```

---

#### Change commit message not pushed

```
git commit --amend -m "New commit message"n
```

## --

#### stash

- To add changes to stash stack

```console
git stash
```

- Shows list of stashed changes

```console
git stash list
```

- Retrieve stas, N.B. git Add . before aplly stash files to prevent errors

```console
git stash apply stash@{0}
```

---
