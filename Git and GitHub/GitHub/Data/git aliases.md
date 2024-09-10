# git aliases

- [[git config#Global Git Config | Global Git Config]]

## Adding Aliases

We can easily set up Git aliases to make our Git experience a bit simpler and faster.
For example, we could define an alias `git ci` instead of having to type `git commit`
Or, we could define a custom `git lg` command that prints out a custom formatted commit log.

    	[alias]
    	s = status
    	l = log

We can also alter configuration variables from the command line if preferred with `git config --global`.

```bash
git config --global alias.showmebranches branch
```

---

## Aliases With Arguments

- Argument are passed automatically after command

```
  [alias]
  s = status
  l = log
  showmebranches = branch
  cm = commit -m
```

```bash
git cm <arg1> <arg2> ...
```

```bash
git cm "my new commit"
```

---

## Resurces

- [Must-have-git-aliases](https://www.durdn.com/blog/2012/11/22/must-have-git-aliases-advanced-examples/)
- [The Ultimate Git Alias Setup](https://gist.github.com/mwhite/6887990)
- [GitHub of gitalias](https://github.com/GitAlias/gitalias)

---
