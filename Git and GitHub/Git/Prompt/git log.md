# git log

- `Q` quit from logging
- `Enter` show one more log into the list
- `--oneline` make logs prettier and less messy

```bash
git log --oneline
```

- result of previous snippet is the follow

```
<commitHash> <commitMessage>
```

---

- `--pretty` gets more options to modify rendering information
  - [DOC of Git-Basics-Viewing-the-Commit-History](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)
  - `%h` Abbreviated commit hash
  - `%cn`Committer name
  - `%s` Subject/Commit message

```bash
git log --pretty=format:"[hash: %h ~ %cn] => %s"
```

- _HASH CODE TO USE IN COMMANDS LINE IS GOTTEN FROM %h_ (Short versione off full commit id; than again commit id includes the hash part that is the first one)

---
