- Docker containers are meant to be transient in nature
	- This means they are meant to last only for a short period of time

- To persist data processed by the container, we attach a volume to the container when it is created

- When volumes are created, the data processed by the container is now placed in the volume thereby retaining it permanently

- Just as with Docker, the pods created in Kubernetes are transient in nature

- To persist data in Kubernetes, we attach a volume to the pod
	- Therefore, even after the pod is deleted, the data remains

- To create a volume on a pod, add a <span style="color:red">volumes</span> property to the pod definition file with the name of the volume added to the <span style="color:red">name</span> property under it
	- The <span style="color:red">volumes</span> property is a sibling of the <span style="color:red">containers</span> property in the pod spec

![[volumes-1.png]]

- Because a volume needs a storage, when creating a volume, you can choose to configure its storage in different ways

- If using a directory on the host as the storage location for the volume, add the <span style="color:red">hostPath</span> property under the <span style="color:red">volumes</span> property on the pod spec
	- Under this, we add the <span style="color:red">path</span> and <span style="color:red">type</span> properties with the location of the path and type of the path respectively

![[volumes-2.png]]

- Once a volume is created, to access it from a container, we mount the volume to a directory inside of the container

- To mount a volume to a directory inside of the container, we add the <span style="color:red">volumeMounts</span> property as a child of the <span style="color:red">containers</span> property in the pod spec
	- Under this, we add the <span style="color:red">mountPath</span> and <span style="color:red">name</span> properties with the location of the path inside of the container and the name of the volume respectively

![[volumes-3.png]]

- The <span style="color:red">hostPath</span> property is fine if the cluster is made up of a single node but is not recommended if using a multi-node cluster
	- This is because the pods will use that directory on all nodes and expect all of them to be the same and have the same data
	- Since they are on different servers they are, in fact, not the same; unless you configure some sort of external replicated cluster storage solution

- There are volume storage solutions for the pod spec in addition to the <span style="color:red">hostPath</span> property that can be configured
	- NFS
	- GlusterFS
	- Flocker
	- FiberChannel
	- CephFS
	- ScaleIO
	- AWS EBS (public cloud solution)
	- Azure Disk or File (public cloud solution)
	- Google Persistent Disk (public cloud solution)

- Each storage solution has a different configuration property for the pod spec
	- Ie. AWS EBS uses the "awsElasticBlockStorage" property with the "volumeID" and "fsType" properties under it

![[volumes-4.png]]