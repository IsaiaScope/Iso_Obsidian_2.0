#### Change default branch
- You need to go to the GitHub page for your forked repository, and click on the “Settings” button.
- Click on the "Branches" tab on the left hand side. There’s a “Default branch” drop-down list near the top of the screen. Where u can change the default branch
- --
####  Creating a new branch & first push
1. creating a new branch locally
```console
git checkout -b  <myNewBranch> 
```
1. first push to create the branch in the repo
```console
git push --set-upstream origin <myNewBranch>
```
---
#### Delete branch & delete remote branch
1. delete ***local*** branch
	- use -D if it's a merged one 
```console
git branch -d <localBranchName> 
```
2. delete ***remote*** branch 
```console
git push origin --delete <remoteBranchName>
```
---





