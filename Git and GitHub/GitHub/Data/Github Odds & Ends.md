# GitHub Odds & Ends

## Public Vs. Private Repos

- _Public repos_ are accessible to everyone on the internet. Anyone can see the repo on Github
- _Private repos_ are only accessible to the owner and people who have been granted access.

---

## READMEs

A README file is used to communicate important information about a repository including:

- What the project does
- How to run the project
- Why it's noteworthy
- Who maintains the project

_If you put a README in the root of your project, GitHub will recognize it and automatically display it on the repo's home page_

READMEs are Markdown files, ending with the .md extension. Markdown is a convenient syntax to generate formatted text. It's easy to pick up!

---

## GitHub Gists

Github Gists are a simple way to share code snippets and useful fragments with others. Gists are much easier to create, but offer far fewer features than a typical Github repository.

---

## GitHub Pages

Github Pages is a hosting service for static webpages, so it does not support server-side code like Python, Ruby, or Node. Just HTML/CSS/JS!

_User Site_
You get one user site per Github account. This is where you could host a portfolio site or some form of personal website. The default url is based on your Github username, following this pattern: username.github.io though you can change this!

_Project Sites_
You get unlimited project sites! Each Github repo can have a corresponding hosted website. It's as simple as telling Github which specific branch contains the web content. The default urls follow this pattern: username.github.io/repo-name

---

# Feature Branch Naming

There are many different approaches for naming feature branches. Often you'll see branch names that include slashes like _bug/fix-scroll_ or _feature/login-form_ or _feat/button/enable-pointer-events_
Specific teams and projects usually have their own branch naming conventions. To keep these slides simple and concise, I'm just going to ignore those best- practices for now.

---

# Pull Requests (PR)

Pull Requests are a feature built in to products like Github & Bitbucket. _They are not native to Git itself_.
They allow developers to alert team-members to new work that needs to be reviewed. They provide a mechanism to approve or reject the work on a given branch. They also help facilitate discussion and feedback on the specified commits.
"I have this new stuff I want to merge in to the master branch...what do you all think about it?"

[[Pull request.png]]

## The Workflow

- Do some work locally on a feature branch
- Push up the feature branch to Github
- Open a pull request using the feature branch just pushed up to Github
- Wait for the PR to be approved and merged. Start a discussion on the PR. This part depends on the team structure.

[[Pull request conflicts.png]]

## Recap

Pull Requests are a fancy way of requesting changes from one branch be merged into another branch.

Tools like GitHub & Bitbucket allow us to generate pull requests via an online interface. Team members can then view the changes and decide to merge them in or reject them. PR's also provide a place to discuss the changes and provide feedback.

---

# Branch Protection Rule

- From GitHub is possible to declare Branch Protection Rule
  - For example consent requiring pull request before merging

---

# Fork & Clone

The "fork & clone" workflow is different from anything we've seen so far. Instead of just one centralized Github repository, every developer has their own Github repository in addition to the "main" repo. Developers make changes and push to their own forks before making pull requests.

It's very commonly used on large open-source projects where there may be thousands of contributors with only a couple maintainers.

## Forking

GitHub (and similar tools) allow us to create personal copies of other peoples' repositories. We call those copies a "fork" of the original.

When we fork a repo, we're basically asking GitHub "Make me my own copy of this repo please"

As with pull requests, forking is not a Git feature. The ability to fork is implemented by GitHub.

### Now What?

Now that I've forked, I have my very own copy of the repo where I can do whatever I want!

I can clone my fork and make changes, add features, and break things without fear of disturbing the original repository.

If I do want to share my work, I can make a pull request from my fork to the original repo. Now What?

### To Summarize!

- I fork the original project repo on Github
- I clone my fork to my local machine
- I add a remote pointing to the original project repo. This remote is often named upstream.
- I make changes and add/commit on a feature branch on my local machine
- I push up my new feature branch to my forked repo (usually called origin)
- I open a pull request to the original project repo containing the new work on my forked repo
- Hopefully the pull request is accepted and my changes are merged in!

[[Fork flow (1).png]]
[[Fork flow (2).png]]

---
