#### Change default branch
- You need to go to the GitHub page for your forked repository, and click on the “Settings” button.
- Click on the "Branches" tab on the left hand side. There’s a “Default branch” drop-down list near the top of the screen. Where u can change the default branch
- --
####  Creating a new branch & first push
1. creating a new branch *locally*
```console
git checkout -b  <myNewBranch> 
```
1. *first push* to create the branch in the repo
	- remember to do the commit first
```console
git push --set-upstream origin <myNewBranch>
```
---
#### Delete branch & delete remote branch
1. delete *local* branch
	- use -D if it's a merged one 
```console
git branch -d <localBranchName> 
```
2. delete *remote* branch 
```console
git push origin --delete <remoteBranchName>
```
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




