# git config

## Global Git Config

Git looks for the global config file at either _~/.gitconfig_ or _~/.config/git/config_. Any configuration variables that we change in the file will be applied across all Git repos.

### Accessing Local config File

- Inside root repo

```bash
cd .git

explorer .

```

- Open config file

---

### Accessing Global config File

```bash
git config --global --edit
```

---

- To configure the name that Git will associate with your work, run this command

```bash
git config --global user.name "Isaia Riva"
```

---

- Do the same thing for your email using the following command. When we get to Github, you'll want your Git email address to match your Github account

```bash
git config --global user.email blah@blah.com
```

---

- retrive email or name value, in general `git config <variable>`

```bash
git config user.email
```

```bash
git config user.name
```

---
