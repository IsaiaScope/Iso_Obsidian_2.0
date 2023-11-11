# Theory

- _Repository_
  - A Git "Repo" is a workspace which tracks and manages files within a folder. Anytime we want to use Git with a project, app, etc we need to create a new git repository. We can have as many repos on our machine as needed, all with separate histories and contents
- _Branches_
  - Branches are an essential part of Git!
  - Think of branches as alternative timelines for a project.
  - They enable us to create separate contexts where we can try new things, or even work on multiple ideas in parallel
  - If we make changes on one branch, they do not impact the other branches (unless we merge the changes)
- _HEAD_
  - HEAD is simply a pointer that refers to the current "location" in your repository. It points to a particular branch reference
  - HEAD refers himself to the last commit of the current branch you are on
  - [[Git+&+Github_+Branching.pdf]] good graphic example how HEAD moves around
```bash
ls -a
cd .git
ls
cat HEAD
// ref: refs/heads/develop

```