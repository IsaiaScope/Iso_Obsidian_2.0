# HEAD

- HEAD is simply a pointer that refers to the current "location" in your repository. It points to a particular branch reference
- _HEAD refers himself to the last commit of the current branch you are on_
- [[Git+&+Github_+Branching.pdf]] good graphic example how HEAD moves around
- [[Git Behind The Scene#.git folder | .git folder]]

---

## What is Really ?

- _is a file text that contains a path_
- to see what HEAD contain
- inside root project when `git init` was lunched

```bash
ls -a // to .git folder that is hidden

cd .git // to enter in .git

ls // to see folder and file list

cat HEAD // to read HEAD value
	// ref: refs/heads/<branch-name>
```

- shortcut could be

```bash
cat .git/HEAD
	// ref: refs/heads/<branch-name>
```

---

### Let's discuss the Result

- `ref: refs/heads/<branch-name>` is the file text path where HEAD refer to
- going to that path starting from the root project folder

```bash
cd .git/refs/heads
ls // <branch1> <branch2> <branch3> ...  (develop  production  test)
```

- _inside the_ `.git/refs/heads` _folder there is a text file for each branch of the project, and each one contains the value of the last commit hash of that specific branch_
- _switching branches and operating with them change the REFERENCE PATH of the HEAD and consequentially the reference to the branch text file_

---
