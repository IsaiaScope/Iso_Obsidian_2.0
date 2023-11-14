# git merge

- Branching makes it super easy to work within self- contained contexts, but often we want to incorporate changes from one branch into another!
- We can do this using the `git merge` command

```bash
git merge <branch-name>
```

---

## Merging Made Easy

Remember these two merging concepts:

- We merge branches, not specific commits
- We always merge to the current HEAD branch

To merge, follow these basic steps:

- Switch to or checkout the branch you want to merge the changes into (the receiving branch)
- Use the git merge command to merge changes from a specific branch into the current branch.

[[Merging.png]]

---

### Fast Forwards Merge

[[This is what merge really looks like.png]]

---

### Not All Merges Are Fast Forwards!

[[Not All Merges Are Fast Forwards.png]]

---

## Resolving Conflicts

Depending on the specific changes your are trying to merge, Git may not be able to automatically merge. This results in merge conflicts, which you need to manually resolve

![[Conflict Markers.png]]

Whenever you encounter merge conflicts, follow these steps to resolve them:

- Open up the file(s) with merge conflicts
- Edit the file(s) to remove the conflicts. Decide which branch's content you want to keep in each conflict. Or keep the content from both.
- Remove the conflict "markers" in the document
- Add your changes and then make a commit!

---

## Update An Old Working Git Branch With Another Branch

```bash
git switch <old_branch_to_update>
```

```bash
git merge <branch_where_take_data_from>
```

---
