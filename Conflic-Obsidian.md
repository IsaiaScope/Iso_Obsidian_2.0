- legion 5 vault directory
```console

 cd C:\Users\iso_o\OneDrive\Desktop\Workspace\Iso_Obsidian_2.0
 
```
- working pc vault directory
```console

 cd C:\Users\isaia.riva\Documents\Workspace\IsoObsidian
 
```
----
##### To resolve conflict problems
1. git command to reset the vault to the current GitHub state
	- a head is **a ref that points to the tip (latest commit) of a branch**
```console

git reset --hard origin HEAD

```
---
2. ***git add .*** stages new files and modifications, **without deletions** (on the current directory and its subdirectories), praticalli the plus button in VS. The ***git commit*** command will save all staged changes, along with a brief description from the user, in a “commit” to the local repository. The ***-m*** stands for message; praticalli the + button in VS
```console

git add .

git commit -m "fixed git things"

git push

```
---

