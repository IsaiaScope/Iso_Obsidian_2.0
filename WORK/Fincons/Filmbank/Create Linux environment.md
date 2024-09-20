---
tags:
  - Fincons/Filmbank
---

# Create Linux environment

## Install Linux

> download _Debian_ or Ubuntu from windows store

move to C disk after user creation, remember the pwd because for lunch most of fallowing commands auth is needed

```bash
cd /mnt/c
```

move to folder project

```bash
cd Users/isaia.riva/Desktop/op/shaka-player
```

```bash
cd Users/isaia.riva/Documents/Workspace/Filmbank/dds-repository-libs-shaka
```

install dependencies

```bash
npm i
```

lists update where packages are taken

```bash
sudo apt update
```

package contents upgrade

```bash
sudo apt upgrade
```

In case of new environment maybe some dependencies are needed
make sure that git is installed like on you machine

```bash
sudo apt install curl

```

```bash
sudo apt-get install python3
```

to launch a file just move the terminal on that, for example for launching `install-linux-prereqs.sh` file use

```bash
build/install-linux-prereqs.sh
```

---

### Note

#### Problem That involve my case

##### python problem

update `build/install-linux-prereqs.sh` at `sudo apt -y install git python default-jre-headless apache2` with `sudo apt -y install git python3 default-jre-headless apache2` adding 3 to python

##### other packages problem in console with node

```
However the following packages replace it:
python-is-python3
E: Package 'python-minimal' has no installation candidate
```

```
The following packages have unmet dependencies:
nodejs : Depends: python-minimal but it is not installable
E: Unmet dependencies. Try 'apt --fix-broken install' with no packages (or specify a solution).
```

launching the next command helps removing some stalling moments and resetting the situation

```bash
sudo apt --fix-broken install
```

---
