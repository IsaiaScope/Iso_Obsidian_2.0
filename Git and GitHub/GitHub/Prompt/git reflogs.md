# git reflogs

Git keeps a record of when the tips of branches and other references were updated in the repo.
We can view and update these reference logs using the git reflog command

[[git reflogs.png]]

- `.git/logs` folder contain a file called HEAD that contains all logs, every time we change branch or make an important operation the logs are updated

## Limitations!

Git only keeps reflogs on your _local_ activity. They are not shared with collaborators.
Reflogs also expire. Git cleans out old entries after around 90 days, though this can be configured.

---

The `git reflog` command accepts subcommands `show`, `expire`, `delete`, and `exists`. Show is the only commonly used variant, and it is the default subcommand.

`git reflog show` will show the log of a specific reference (it defaults to HEAD).

For example, to view the logs for the tip of the main branch we could run `git reflog show main`.

```bash
git reflog show <branch-name>
```

```bash
git reflog show HEAD
```

---

## Reflog Output

![[Reflog Output.png]]

### Reflog References

We can access specific git refs is `name@{qualifier}`. We can use this syntax to access specific ref pointers and can pass them to other commands including checkout, reset, and merge.

- `qualifier` could be:
  - a number of move
  - a date or time stamp

```bash
name@{qualifier}
```

#### Timed References

Every entry in the reference logs has a timestamp associated with it. We can filter reflogs entries by time/date by using time qualifiers like

- 1.day.ago
- 3.minutes.ago
- yesterday
- Fri, 12 Feb 2021 14:06:21 -0800

```bash
git reflog master@{one.week.ago}
```

```bash
git checkout bugfix@{2.days.ago}
```

```bash
git diff main@{0} main@{yesterday}
```

```bash
git diff HEAD@{0} HEAD@{2}
```

```bash
git diff master master@{yesterday}
```

```bash
git reflog show HEAD@{two.days.ago}
```

---

## Rescuing Lost Commits With Reflog

We can sometimes use reflog entries to access commits that seem lost and are not appearing in git log.

- Execute `git reflog show <brach>`

```bash
git reflog show master
```

- Chose from output witch log are you interest in

![[Rescuing Lost Commits With Reflog.png]]

- is possible to use:

  - _commit hash (fb5072a)_
  - _master@{1}_

- with _commit hash_ we can check it out and a new branch could be created from that detached HEAD and then you can do what you want like merging that into another or developing more stuff o retrive some needed information

```bash
git checkout <commit-hash>
```

```bash
git switch -c <new-branch>
```

- with _master@{1}_ or _commit hash_ we can reset
  - [[git reset]]

```bash
git reset --hard master@{1}
```

### Undoing A Rebase w/ Reflog - It's A Miracle!

- From a rebase for cleaning up history for example

```bash
git rebase -i HEAD~2
```

- the process is the same as before
- Little Note: if you manipulate the history with reflog maybe squashing more commits together, when undo the rebase al commit history is going back because every commit contain a parent reference

---
