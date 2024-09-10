# git clone

## Cloning

We can clone a remote repository hosted on Github or similar websites. All we need is a URL that we can tell Git to clone for use

To clone a repo, simply run `git clone <url>`.

Git will retrieve all the files associated with the repository and will copy them to your local machine.

In addition, Git initializes a new repository on your machine, giving you access to the full Git history of the cloned project

```bash
git clone <url>
```

---

### We are not limited to Github Repos!

`git clone` is a standard git command.
It is NOT tied specifically to Github. We can use it to clone repositories that are hosted anywhere! It just happens that most of the hosted repos are on Github these days

---

## SSH Keys

You need to be authenticated on Github to do certain operations, like pushing up code from your local machine. Your terminal will prompt you every single time for your Github email and password, unless...

You generate and configure an SSH key! Once configured, you can connect to Github without having to supply your username/password.

- [connecting-to-github-with-ssh](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

---
