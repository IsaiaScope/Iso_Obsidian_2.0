# DOCS

- [Site of GitHub](https://docs.github.com/en)
- [[git remote#Remote | Remote]]
- [[git remote#Origin?! Remote Default Name (Customizable) | Origin?! Remote Default Name (Customizable)]]
- [[Remote Branches]]

# Prompt

## Git Bash

- [[Git and GitHub/Git/Summary#Git Bash | Git Prompt]]

### Git CLI for GitHub Interaction

| Command           | Meaning                                                                      |                  Link                   |
| ----------------- | ---------------------------------------------------------------------------- | :-------------------------------------: |
| `git clone`       | clone a remote repository hosted on Github                                   |    [:TiFileSymlink:](git%20clone.md)    |
| `git remote`      | manage set of tracked repositories, set and recover remote name `git remote` |   [:TiFileSymlink:](git%20remote.md)    |
| `git push`        | send local work up to Github Repo                                            |    [:TiFileSymlink:](git%20push.md)     |
| `git fetch`       | update remote branch                                                         |    [:TiFileSymlink:](git%20fetch.md)    |
| `git pull`        | fetch and integrate remote branch into local branch                          |    [:TiFileSymlink:](git%20pull.md)     |
| `git cherry-pick` | apply the changes introduced by some existing commits                        | [:TiFileSymlink:](git%20cherry-pick.md) |

---

### git fetch & git pull

| git fetch                                                 |                            git pull                             |
| --------------------------------------------------------- | :-------------------------------------------------------------: |
| Gets changes from remote branch(es)                       |               Gets changes from remote branch(es)               |
| Updates the remote-tracking branches with the new changes | Updates the current branch with the new changes, merging them i |
| Does not merge changes onto your current HEAD branch      |                  Can result in merge conflicts                  |
| Safe to do at anytime                                     |         Not recommended if you have uncommitted changes         |

---

### Common Problems

| Command                                                  |                                              Link                                              |
| -------------------------------------------------------- | :--------------------------------------------------------------------------------------------: |
| _Make a Repo_                                            |                             [:TiFileSymlink:](Make%20a%20Repo.md)                              |
| _Change default branch_                                  |                           [[git init#Change default branch \| Link]]                           |
| _Create Local and Remote Branch_                         |                  [:TiFileSymlink:](Create%20Local%20and%20Remote%20Branch.md)                  |
| _Delete Local and Remote Branch_                         |                  [:TiFileSymlink:](Delete%20Local%20and%20Remote%20Branch.md)                  |
| _Migrate Repo to Another Url_                            |                   [:TiFileSymlink:](Migrate%20Repo%20to%20Another%20Url.md)                    |
| _Check Url Repo or Directory where Local Repo is Cloned_ | [:TiFileSymlink:](Check%20Url%20Repo%20or%20Directory%20where%20Local%20Repo%20is%20Cloned.md) |
| _Create a New Local Branch from a specific commit_       |            [[git switch#Create a New Local Branch from a specific commit \| Link]]             |

---

## Workflow Recap

![[Workflow Recap.png]]

---
