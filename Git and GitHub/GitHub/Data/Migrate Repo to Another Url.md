# Migrate Repo to Another Url

- This process change also reference in the project so next pull/push actions are gonna be on new Url
  - `--mirror` sets up a mirror of the source repository and copy all history, note and ecc...

```bash
git clone --mirror <URL OLD>

cd <directory where your OLD repo was cloned>

git remote set-url origin <NEW URL>

git push --mirror origin
```

---
