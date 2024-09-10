# git restore

`git restore` is a brand new Git command that helps with undoing operations.

Recall that `git checkout` does a million different things, which many git users find very confusing. `git restore` was introduced alongside `git switch` as alternatives to some of the uses for checkout.

## Unmodifying Files with Restore

Suppose you've made some changes to a file since your last commit. You've saved the file but then realize you definitely do NOT want those changes anymore

_To restore the file to the contents in the HEAD_, use `git restore <file-name>`

```bash
git restore <path/file-name>
```

---

- Unmodifying All Files

```bash
git restore .
```

---

### NOTE

The above commands is not "undoable" If you have uncommited changes in the file, they will be lost

---

`git restore <file-name>` _restores using HEAD as the default source_, but we can change that using the `--source` option

- For example, `git restore --source HEAD~1 home.html` will restore the contents of home.html to its state from the commit prior to HEAD. You can also use a particular commit hash as the source
  - [[HEAD ~]]
  - [[HASH]]

```bash
git restore --source HEAD~1 app.js
```

---

## Unstaging Files with Restore

if you have accidentally added a file to your staging area with `git add` and you don't wish to include it in the next commit, you can use `git restore` to remove it from staging.

- Use the `--staged` option like this: `git restore --staged app.js`

```bash
git restore --staged <path/file-name>
```

---

- Unstaging All Files

```bash
git restore --staged .
```

---
