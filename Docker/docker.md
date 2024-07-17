[Docker YT - Theory](https://www.youtube.com/watch?v=Ud7Npgi6x8E)

## virtualisation

- [[Pasted image 20240717125827.png | virtualisation]]

_host_ could be anything it could be your local PC it could a server up in the cloud and whatever it is it's a piece of Hardware, what happens is we take little pieces of each of these pieces of hardware (CPU, memory, I/O - hard-drive) and separate them out into a separate machine this is a _virtual machine_.
then we take these pieces of hardware and in this virtual machine we actually run a full entire operating system.
this technique is commonly used in the cloud if you're deploying something to AWS or ec2 typically what you're doing is you're spinning up a new virtual machine that you can then deploy your code onto.

## containerization 
which is the thing that Docker is kind of based around, in container setup what you would do is you would have a host PC much like the virtualization setup that we set up before now let's say on this host PC we want to run a set of processes but we want these processes to run in isolation we don't want them to touch anything
else now we can achieve that using some technics like the _CH root command_ which will create a new route for a process so it can only live inside that
root and it can't touch anything outside of that like any of the other users directories or things like that that are already on the system.  We could also use a kernel feature like the r 