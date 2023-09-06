- A Docker host has a set of its own processes running on it (i.e. a number of operating system processes, the docker daemon itself, the SSH server, etc…)

![[psd-1.png]]

- *Remember: unlike virtual machines, containers are not completely isolated from their host

![[psd-2.png]]

- Containers and their host share the same kernel

- Containers are isolated using namespaces in Linux

- The host has a namespace and the containers have their own namespace

![[psd-3.png]]

- All processes run by a container are in fact run on the host itself but in their own namespace
	- As far as the Docker container is concerned, it is in its own namespace and it can see its own processes only
	- It cannot see anything outside of it or in any other namespace

- When you list processes within the Docker container, you see the processes with their process IDs

![[psd-4.png]]

- For the Docker host, all processes of its own, as well as those in the child namespaces are visible as just another process in the system

- When you list the processes on the host, you see a list of processes and there IDs
	- These will show the same processes as the container (including more) but with different process IDs

![[psd-5.png]]

- Processes can have different process IDs in different namespaces and this is how Docker isolates containers within a system

- The Docker host has a set of users: a root user and a slew of non-root users

- By default, Docker runs processes within containers as the root user

- Both in the container and outside of the container as the host, the process is run as the root user

- If you don't want processes within the container to run as the root user, you may set the user using the <span style="color:red">kubectl --user</span> flag within the Docker run command
	- You will see that the process now runs with the new user ID

![[psd-6.png]]

- Another way to enforce user security in containers is to define the user in the images themselves at the time of creation

- To define the user in the image, use the USER instruction in the Dockerfile then give it the user ID, then build the custom image

![[psd-7.png]]

- When the user is specified within the image, you can run the container without specifying the user and when you output the processes, you will see the user is the user specified at creation time

![[psd-8.png]]

- Docker implements a set of security features that limit the abilities of the root user within the container
	- This means that the root user within the container isn't really like the root user on the host

- *Remember: the root user is usually the most powerful user on a system and can do literally anything
	- And so can a process run by the root user

![[psd-9.png]]

- On a Linux operating system, you can see a full list of root user capabilities at /usr/include/linux/capability.h

- By default, Docker runs a container with a limited set of capabilities
	- Thus the processes run within the container do not have the privileges to do certain things that require full access

![[psd-10.png]]

- If you wish to override the default behavior of the Docker container and provide additional privileges than what is available, then use the "cap-add" option in the `docker run` command

![[psd-11.png]]

- Similar to adding privileges to a container user, you drop privileges using the "cap-drop" option with the `docker run` command

![[psd-12.png]]

- If you wish to run the container with all privileges available, use the "--privileged" flag with the `docker run` command