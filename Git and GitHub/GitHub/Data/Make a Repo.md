# Make a Repo

## Push an existing repository from the command line

### Adding A New Remote

A remote is really two things: a URL and a label. To add a new remote, we need to provide both to Git.

```bash
git remote add origin <url>
```

_Okay Git, anytime I use the name "origin", I'm referring to this particular Github repo URL._

Try viewing your remotes with `git remote -v`, and you should now see a remote showing up! _Remember, by setting up a remote we are just telling Git about a remote repository URL. We have not "communicated" with the Github repo at all yet_.

#### Origin? & Custom Origin Name

Origin is a conventional Git remote name, but it is not at all special. It's just a name for a URL.

When we clone a Github repo, the default remote name setup for us is called origin. _You can change it_. Most people leave it.

```bash
git remote add <my-custom-origin-name> <url>
```

---