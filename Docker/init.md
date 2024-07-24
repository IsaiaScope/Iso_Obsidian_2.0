[Docker YT - Theory](https://www.youtube.com/watch?v=Ud7Npgi6x8E)
[Docker YT - Theory in Deept](https://www.youtube.com/watch?v=pg19Z8LL06w)

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

## images

![[Pasted image 20240718122852.png]]
![[Pasted image 20240718123044.png]]
![[Pasted image 20240718123114.png]]
![[Pasted image 20240718123148.png]]
![[Pasted image 20240718123448.png]]
Images registry - Docker hub
![[Pasted image 20240718123605.png]]
![[Pasted image 20240718123633.png]]
![[Pasted image 20240718123813.png]]

_Images are useful just if running in a container environment_

![[Pasted image 20240718154102.png]]

## Port Binding

![[Pasted image 20240718154413.png]]
![[Pasted image 20240718154900.png]]

## Registry vs Repository

![[Pasted image 20240719114649.png]]
![[Pasted image 20240719114724.png]]

## prompt

### images

docker images = List all Docker images
docker pull {name}: {tag/version} = Pull an image from a registry
docker image rm [image id]
--force if image is used in some container

docker run {name}:{tag/version} = _Creates_ a container from given image and starts it, image can be also in the cloud and docker install that before executing in automatic
-d or --detach = Runs container in background and prints the container ID, prevent terminal to lock and keep working in it
-p or --publish = Publish a container's port to the host
-p {HOST_PORT}: {CONTAINER_PORT}
docker run -d -p 9000:80 nginx:1.23 = expose container at localhost 9000, Standard to use the same port on your host as container is using
-a or --all = Lists all containers (stopped and running)
--name = Assign a name to the container
-e = set environment variable
--net = chose the network where to run

_NOTE_ every docker run create a new container every time even they are not running they are persisting

### containers

docker ps = List running containers

docker logs {container ID/name} = View logs from service running inside the container. (which are present at the time of execution)

docker stop {container ID/name container ID/name container ID/name ....} = Stop one or more running containers

docker start {{container ID/name container ID/name container ID/name ....}} = Start one or more stopped containers

docker exec -it {container ID/name} {directory - _/bin/bash_ for the root }= get it (interactive terminal) for specified container from there is possible to execute normal commands to navigare and create folders. type `exit` to exit terminal

---

## Install

- [Debian](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-debian-10)

### General Docker Management

If you need to start or check the status of the Docker service on a system that uses `systemd`, you can use the following commands:

**Start/Stop Docker Service**:
NOTE: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?

```bash
sudo service docker start/stop
```

**Check Docker Service Status**:

```bash
sudo service docker status
```

## Building

![[Pasted image 20240719114844.png]]
![[Pasted image 20240719114932.png]]

### Dockerfile

![[Pasted image 20240719115321.png]]
![[Pasted image 20240719115342.png]]
![[Pasted image 20240719115422.png]]
we start from node and build our image on top of that adding more data and ecc...
![[Pasted image 20240719115525.png]]
![[Pasted image 20240719120205.png]]

![[Pasted image 20240719120432.png]]
![[Pasted image 20240719120448.png]]
![[Pasted image 20240719120523.png]]
NOTE /app/ the slash at the and say to create the folder if doesn't exist

![[Pasted image 20240719120622.png]]
set the path as default location
![[Pasted image 20240719120726.png]]

### build an Image from docker file

`docker build -t <name> <dockerfile location>`
-t or --tag = Sets a name and optionally a tag in the "name:tag" format
`docker build -t node-app:1.0 .`

than run `doker images` if build success the nel image is available to run
docker run for starting a new container

_NOTE_ in case of https://gitlab.com/nanuchi/docker-in-1-hour
`docker run -d -p 3000:3000 node-app: 1.0` the second 3000 is where the server is running on the container and we expose that on our local host 3000 define by the first one

## Docker network
https://gitlab.com/nanuchi/techworld-js-docker-demo-app
![[Pasted image 20240722175649.png]]

pulling images
`docker pull mongo`
`docker pull mongo-express`

![[Pasted image 20240722175842.png]]

docker network ls = give default network created by docker
`docker network create <name>` = create a new docker network
when running docker run --net flag is available to take a network for running the app

![[Pasted image 20240723091715.png]]
always look up how to set images and environment variables on official doc on Docker Hub 
![[Pasted image 20240723105432.png]]

## Docker Compose
