# git diff

We can use the git diff command to view changes between commits, branches, files, our working directory, and more!
We often use git diff alongside commands like git status and git log, to get a better picture of a repository and how it has changed over time.

- Without additional options, git diff lists all the changes in our working directory that are _NOT staged_ for the next commit.
  - [[Commit flow.png]]

```bash
git diff
```

---

- `git diff --staged` or `--cached` will list the changes between the staging area and our last commit.
- "Show me what will be included in my commit if I run git commit right now"

```bash
git diff --staged
```

---

- `git diff HEAD` lists _all changes_ in the working tree since your last commit.

```bash
git diff HEAD
```

---

## Git Diff Output

- [[Git+&+Github_+Diffs.pdf]]

---

## Diff-ing Specific Files

We can view the changes within a specific file by providing git diff with a filename.

```bash
git diff HEAD <path/file-name>
```

```bash
git diff --staged <path/file-name>
```

---

## Comparing Branches

- `git diff branch1..branch2` will list the changes between the tips of branch1 and branch2
- show differences of all files

```bash
git diff branch1..branch2
```

---

## Comparing Commits

- To compare two commits, provide git diff with the _commit hashes_ of the commits in question.
  - [[HASH]]

```bash
git diff commit1..commit2
```

---
