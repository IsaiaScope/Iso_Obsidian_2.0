#### delete ==local== branch that is unmerged
- use -D if it's a merged one
```console
git branch -d <localBranchName> 
```
---
#### delete branch ==remotely==
```console
git push origin --delete <remoteBranchName>
```