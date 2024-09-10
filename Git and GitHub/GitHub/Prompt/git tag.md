# git tag

## Tags

Tags are pointers that refer to particular points in Git history. We can mark a particular moment in time with a tag.
Tags are most often used to mark version releases in projects (v4.1.0, v4.1.1, etc.)

_Think of tags as branch references that do NOT CHANGE. Once a tag is created, it always refers to the same commit. It's just a label for a commit._

## The Two Types

There are two types of Git tags we can use: _lightweight and annotated tags_

- _lightweight tags_ are...lightweight. They are just a name/label that points to a particular commit.
- _annotated tags_ store extra meta data including the author's name and email, the date, and a tagging message (like a commit message)

### Semantic Versioning

The semantic versioning spec outlines a standardized versioning system for software releases. It provides a consistent way for developers to give meaning to their software releases (how big of a change is this release??)

- Semantic versioning outlines
  - [[Releases Versioning.png]]
  - [DOC of Semantic versioning outlines](https://semver.org/)

---

## Viewing Tags

- `git tag` will print a list of all the tags in the current repository

```bash
git tag
```

---

- We can search for tags that match a particular pattern by using `git tag -l` and then passing in a wildcard pattern. For example, `git tag -l "*beta*"` will print a list of tags that include "beta" in their name.

```bash
git tag -l "*beta*"
```

---

- To view the state of a repo at a particular tag, we can use `git checkout <tag>`. This puts us in detached HEAD!

```bash
git checkout <tag>
```

---

## Creating Lightweight Tags

- To create a lightweight tag, use `git tag <tagname>` By default, Git will create the tag referring to the commit that HEAD is referencing.

```bash
git tag <tagname>
```

---

## Annotated Tags

- Use `git tag -a` to create a new annotated tag. Git will then open your default text editor and prompt you for additional information.
- Similar to git commit, we can also use the `-m` option to pass a message directly and forgo the opening of the text editor

```bash
git tag -a <tagname>
```

---

## Tagging Previous Commits

- So far we've seen how to tag the commit that HEAD references. We can also tag an older commit by providing the commit hash: `git tag -a <tagname> <commit-hash>`

```bash
git tag <tagname> <commit>
```

---

## Forcing Tags

- Git will yell at us if we try to reuse a tag that is already referring to a commit. If we use the `-f` option, we can FORCE our tag through.
- Can be use to move a tag from a commit to an another adding `<commit>` parameter

```bash
git tag -f <tagname>
```

---

## Deleting Tags

- To delete a tag, use `git tag -d <tagname>`

```bash
git tag -d <tagname>
```

---

## Pushing Tags

- By default, the `git push` _command doesn’t transfer tags to remote servers_. If you have a lot of tags that you want to push up at once, you can use the `--tags` option to the `git push` command. This will transfer all of your tags to the remote server that are not already there.

```bash
git push --tags
```

- if just a tag need to be pushed to origin could be used

```bash
git push <remote> <tag-name>
```

---

## Comparing Tags With Git Diff

- great for check what change between tag (in this case tags are used to keep tracking of project version releases)

```bash
git diff v17.0.0 v17.0.1
```

---
