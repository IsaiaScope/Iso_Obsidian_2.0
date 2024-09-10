# git cherry-pick

- useful to add a specific commit to a branch
- get hash by lunching git log in original branch where u want to take commit hash code
  - [[HASH]]
- positioning yourself on the branch to update with the hash code of the commit and lunch cherry-pick
- cherry-pick make automatic commit; to avoid that put `-n` flag

```bash
git cherry-pick <commit-hash>
```

---
