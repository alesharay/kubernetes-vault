- Previously when we created volumes, we configured them within the pod definition file so that all configuration information required to configure storage for the volume goes inside of the pod definition file as well

- With using just volumes, when you have a large environment with a lot of users deploying a lot of pods, the users would have to configure storage every time for each pod
	- This means, whatever storage solution is used, the user who deploys a pod would have to configure that solution on all definition files in their own environment every time a change is to be made

- It is preferred that storage is managed centrally by an admin creating a large pool of storage and the users carving out the required storage resources from the pool
	- This is where persistent volumes can help us

![[pv-1.png]]

- A persistent volume (PV) is a cluster-wide pool of storage volumes configured by an administrator to be used by users deploying applications on the cluster
	- PVs are volume plugins like Volumes, but have a lifecycle independent of any individual Pod that uses the PV.
	- This API object captures the details of the implementation of the storage, be that NFS, iSCSI, or a cloud-provider-specific storage system.

- Users can select storage from the volume pool using persist volume claim (PVC)

- As with all objects in Kubernetes, persistent volumes are created using definition files

![[pv-2.png]]

- Under the spec section of the persistent volume definition file, specify the following:
	- <span style="color:red">accessModes</span> = defines how a volume should be mounted on the host
	- <span style="color:red">capacity</span> = specifies the amount of storage to be reserved for the persistent volume
	- Volume type = this configuration will look like the <span style="color:red">volumes</span> config that we saw in the pod template
		- Ie. <span style="color:red">hostPath</span> or <span style="color:red">awsElasticBlockStorage</span>

![[pv-3.png]]

- The available "accessModes" for a persistent volume are:
	- ReadOnlyMany
	- ReadWriteOnce
	- ReadWriteMany

- After creating the PV using kubectl, use the <span style="color:red">kubectl get persistentvolume</span> command to view the newly created PV