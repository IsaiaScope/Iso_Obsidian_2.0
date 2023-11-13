# git remote

## Remote

Before we can push anything up to Github, we need to tell Git about our remote repository on Github. We need to setup a "destination" to push up to

In Git, we refer to these "destinations" as remotes. _Each remote is simply a URL where a hosted repository lives_.

---

### Viewing Remotes

To view any existing remotes for you repository, we can run `git remote` or `git remote -v` (verbose, for more info)
This just displays a list of remotes. If you haven't added any remotes yet, you won't see anything!

```bash
git remote -v
```

---

### Adding A New Remote

_A remote is really two things: a URL and a label_. To add a new remote, we need to provide both to Git.

```bash
git remote add origin <url>
```

_Okay Git, anytime I use the name "origin", I'm referring to this particular Github repo URL._

Try viewing your remotes with `git remote -v`, and you should now see a remote showing up! _Remember, by setting up a remote we are just telling Git about a remote repository URL. We have not "communicated" with the Github repo at all yet_.

#### Origin?! Remote Default Name (Customizable)

Origin is a conventional Git remote name, but it is not at all special. It's just a name for a URL.

When we clone a Github repo, the default remote name setup for us is called origin. _You can change it_. Most people leave it.

```bash
git remote add <my-custom-origin-name> <url>
```

---

## [[Make a Repo]]

---

## Other commands

They are not commonly used, but there are commands to rename and delete remotes if needed.

```bash
git remote rename <old> <new>
```

```bash
git remote remove <name>
```

---
