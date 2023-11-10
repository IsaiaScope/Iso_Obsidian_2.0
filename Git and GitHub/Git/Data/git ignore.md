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
- `.DS_Store` will ignore files named .DS_Store
- `folderName/` will ignore an entire directory
- `*.log` will ignore any files with the .log extension
