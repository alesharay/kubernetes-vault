- Storage drivers help manage storage on images and containers

- If you want to persist storage, you must create a volume

- Volumes are not handled by storage drivers, they are handled by volume driver plugins

- The default volume driver plugin  is local

- The local volume driver plugin helps create a volume on the Docker host and store its data under the /var/lib/docker/volumes directory

- Other volume driver plugins that allow you to create volumes on third-party solutions include:
	- Azure file storage
	- Convoy
	- Digital Ocean block storage
	- Flocker
	- Google computer persistant disk
	- GlusterFS
	- NetApp
	- RexRay
	- Portworx
	- VMWare vSphere  Storage
	- Etc…

- Some volume driver plugins support different storage providers
	- Ie. RexRay can be used to provision storage on AWS EBS, S3, EMC storage arrays (like iselon and scaleIO), Google persistent disk or Open stack cinder

- When you run a docker container, you can choose to use a specific volume driver

![[volume-driver-1.png]]

- Using a specific volume driver will create a container and attach a volume from the chosen driver solution

- On containers with chosen volume drivers, when the container exits, the data is safely stored in the location of that specific driver