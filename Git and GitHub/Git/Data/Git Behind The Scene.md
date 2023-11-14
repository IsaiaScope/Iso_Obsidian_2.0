# Git Behind The Scene

## .git folder

- _Config file_
  - The config file is for...configuration. We've seen how to configure global settings like our name and email across all Git repos, but we can also configure things on a per-repo basis.
- _Refs Folder_
  - Inside of refs, you'll find a heads directory. _refs/heads_ contains one file per branch in a repository. Each file is named after a branch and contains the hash of the commit at the tip of the branch.
  - For example refs/heads/master contains the commit hash of the last commit on the master branch.
  - Refs also contains a refs/tags folder which contains one file for each tag in the repo.
- _HEAD file_
  - HEAD is just a text file that keeps track of where HEAD points.
  - If it contains _refs/heads/master_, this means that HEAD is pointing to the master branch.
  - _In detached HEAD, the HEAD file contains a commit hash_ instead of a branch reference
- _Index file_
  - The index file is a binary file that contains a list of the files the repository is tracking. It stores the file names as well as some metadata for each file.
  - Not that the index does NOT store the actual contents of files. It only contains references to files.
- _Objects Folder_
  - The objects directory contains all the repo files. This is where Git stores the backups of files, the commits in a repo, and more.
  - The files are all compressed and encrypted, so they won't look like much!
  - Contains 4 Types of Git Objects
    - _commit_
    - _tree_
    - _blob_
    - _annotated tag_

![[hidden git folder.png]]

### 4 Types of Git Objects

- Check for graphic view
  - [[Git&+Github_Git+Behind+The+Scenes.pdf]]

> _blobs_
> Git blobs (binary large object) are the object type Git uses to store the contents of files in a given repository. Blobs don't even include the filenames of each file or any other data. They just store the contents of a file!

> _tree_
> Trees are Git objects used to store the contents of a directory. Each tree contains pointers that can refer to blobs and to other trees.
> Each entry in a tree contains the SHA-1 hash of a blob or tree, as well as the mode, type, and filename

> _commit_
> Commit objects combine a tree object along with information about the context that led to the current tree. Commits store a reference to parent commit(s), the author, the commiter, and of course the commit message!
> When we run git commit, Git creates a new commit object whose parent is the current HEAD commit and whose tree is the current content of the _index_ (see above).

> _annotated tag_ > [[git tag | Tags]]

---
