# git switch

_You cannot switching branches with unstaged changes that are in conflict with the branch that you are switching to, but if there are no conflicts between branches files is possible to switch with unstaged changes_

## Switching Branches

- Once you have created a new branch, use `git switch <branch-name>` to switch to it

```bash
git switch <branch-name>
```

---

### Another way of switching??

- Historically, we used` git checkout <branch-name>` to switch branches. This still works.
- The _checkout command_ does a million additional things, so the decision was made to add a standalone switch command which is much simpler
- You will see older tutorials and docs using checkout rather than switch. _Both now work_.

```bash
git checkout <branch-name>
```

---

# Creating & Switching

- Use `git switch` with the `-c` flag to create a new branch _locally_ from the HEAD and switch to it all in one go.
  - Remember `-c `as short for "create"

```bash
git switch -c <branch-name>
```

---

## Another way of Creating & Switching??

- Use `git checkout` with the `-b` bring the same result of `git switch -c <branch-name>`.
  - Remember `-b `as short for "branch"

```bash
git checkout -b  <branch-name>
```

---

