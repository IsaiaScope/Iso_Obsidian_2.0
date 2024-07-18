[Docker YT - Theory](https://www.youtube.com/watch?v=Ud7Npgi6x8E)

## virtualisation

- [[virtualization.png| virtualisation]]

_host_ could be anything it could be your local PC it could a server up in the cloud and whatever it is it's a piece of Hardware, what happens is we take little pieces of each of these pieces of hardware (CPU, memory, I/O - hard-drive) and separate them out into a separate machine this is a _virtual machine_.
then we take these pieces of hardware and in this virtual machine we actually run a full entire operating system.
this technique is commonly used in the cloud if you're deploying something to AWS or ec2 typically what you're doing is you're spinning up a new virtual machine that you can then deploy your code onto.

---

## containerization

_Docker does that by packaging an application into something called a container that has everything the application needs to run like the
application code itself its libraries and dependencies but also the runtime and environment configuration so application and its running environment are both packaged in a single Docker package which you can easily share and distribute_

which is the thing that Docker is kind of based around. In container setup; what you would do is you would have a host PC much like the virtualization setup that we set up before, now let's say on this host PC we want to run a set of processes but we want these processes to run in isolation we don't want them to touch anything
else now we can achieve that using some technics like the _CH root command_ which will create a new root for a process so it can only live inside that
root and it can't touch anything outside of that like any of the other users directories or things like that that are already on the system. We could also use a kernel feature like the _rlimit_ feature which will limit the amount of resources these processes take up. Those techniques amongst other things compass what is containerization. With containerization you could do all this manually yourself but it's really difficult and pretty tricky so there are programs that help manage the life cycle of your containers this is where Docker comes into play. _Docker is a program that manages the life cycles of containers edit them, run them and interact with them. them so to sum up containerization is the ability to create a lightweight environment where processes can run on a host operating system they share all the
same things in that operating system but they cannot touch anything outside of their little bounded box_

## Theory

![[how an OS is made up.png]]
![[Pasted image 20240718113901.png]]
![[Pasted image 20240718113959.png]]
Docker operates just to Application level so It's needs is Linux environment to run, instead VMs have inside also the Kernel (basically window, Linux and OS system in general). This way the images are very tiny and fast
![[Pasted image 20240718114025.png]]
To make possible to run Linux based container on windows tool like Docker Desktop are necessary
![[Pasted image 20240718114134.png]]

---

## Install

- [Debian](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-debian-10)

### General Docker Management

If you need to start or check the status of the Docker service on a system that uses `systemd`, you can use the following commands:

**Start Docker Service**:

```bash
sudo service docker start
```

**Check Docker Service Status**:

```bash
sudo service docker status
```
