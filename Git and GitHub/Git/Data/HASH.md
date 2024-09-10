# HASH

## Commit Hash

Remember, you can use the `git log` command to view commit hashes. We just _need the first 7 digits of a commit hash_

```bash
git log
```

- _commit hashes_ can be acquired with `git log --oneline`

```bash
git log --oneline
```

---

## We need to talk about hashing

### HASHING FUNCTION (SHA-1)

- Git uses a hashing function called SHA-1
- SHA-1 always generates 40-digit hexadecimal numbers
- The commit hashes we've seen a million times are the output of SHA-1
- Git uses SHA-1 to hash our files, directories, and commits.

[[HASHING  FUNCTIONS.png]]

#### Hashing isn't used just for Commits!

- Git Database
  - Git is a _key-value data store_. We can insert any kind of content into a Git repository, and Git will hand us back a unique key we can later use to retrieve that content.
  - These keys that we get back are SHA-1 checksums.
  - [[Git Database.png]]

#### Let's Try Hashing

The` git hash-object` command takes some data, stores in in our .git/objects directory and gives us back the unique SHA-1 hash that refers to that data object.
In the simplest form (shown on the right), Git simply takes some content and returns the unique key that WOULD be used to store our object. But it does not actually store anything

```bash
git hash-object <file>
```

- Read more about Hashing files
  - [[Git&+Github_Git+Behind+The+Scenes.pdf]]

---
