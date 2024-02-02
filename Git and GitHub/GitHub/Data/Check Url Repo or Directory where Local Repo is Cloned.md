# Check Url Repo or Directory where Local Repo is Cloned

- Check Url Repo

```bash
git remote show origin
```

---

- Check directory where local repo-clone is

```bash
git rev-parse --show-toplevel
```

---

## Set New Origin

```bash
git remote set-url origin <repo-url>
```

### SAML SSO (ERROR)

> _CONSOLE ERROR_
> remote: The 'MDS-Technology-Transformation' organization has enabled or enforced SAML SSO.
> remote: To access this repository, you must re-authorize the OAuth Application 'Git Credential Manager'.
> fatal: unable to access 'https://github.com/MDS-Technology-Transformation/videoai-advisor-fe.git/': The requested URL returned error: 403

- Create a token in https://github.com/settings/tokens

```bash
git remote set-url origin https://username:token@github.com/<username or organization>/repository.git
```

- Click on the link in console and allow access using the browser
- All right setup is done

---
