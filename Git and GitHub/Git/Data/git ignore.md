# git ignore

- [Ignoring-files](https://docs.github.com/en/get-started/getting-started-with-git/ignoring-files)
- [DOC of gitignore](https://git-scm.com/docs/gitignore)

## Ignoring Files

- We can tell Git which files and directories to ignore in a given repository, using a .gitignore file. This is useful for files you know you NEVER want to commit, including:
- Secrets, API keys, credentials, etc.
- Operating System files (.DS_Store on Mac)
- Log files
- Dependencies & packages

---

## .gitignore

- _Create a file called .gitignore in the root of a repository_. Inside the file, we can write patterns to tell Git which files & folders to ignore:
- `DS_Store.txt` will ignore files named DS_Store.txt
- `folderName/` will ignore an entire directory
- `*.log` will ignore any files with the .log extension
- `hello.*` matches any file or directory whose name begins with `hello.`
- The pattern `doc/frotz` and `/doc/frotz` have the same effect in any `.gitignore`

---

##  Ignore a file that is already checked in

- If you want to ignore a file that is already checked in, you must untrack the file before you add a rule to ignore it. From your terminal, untrack the file.
- The command with flag `git rm --cached` removes the file from the index but leaves it in the working directory. This indicates to `git` that you don't want to track the file any more.

- change `.gitignore` file and execute the following snippet

```bash
git rm --cached FILENAME
```

- add all and do the commit

```bash
git add . && git commit -m 'commit message'
```

- from now changes to that file becomes just locals
