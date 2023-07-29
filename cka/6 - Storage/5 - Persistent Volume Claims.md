- Persistent volumes and persistent volume claims are two separate objects in the Kubernetes namespace

- Administrators create persistent volumes

- Users create persistent volume claims

- Once PVCs are created, Kubernetes binds PVs to PVCs based on requests and properties set on the volume

- Every PVC is bound to a single PV

![[pvc-1.png]]

- During the binding process, Kubernetes tries to find a PV with the sufficient capacity requested by the PVC along with any other request properties such as accessModes, volume modes, storageclass, etc…

- If there are multiple possible PV matches for a single PVC, and you would like to specifically use a particular volume, you can use labels and selectors to bind to the right volumes

![[pvc-2.png]]

- NOTE: a smaller PVC may get bound to a larger PV if all other criteria matches and there are no better options

- There is a one-to-one relationship between PVCs and PVs, thus no other PVC can utilize the remaining capacity in the PV

![[pvc-3.png]]

- If there are no PVs available, the PVC will remain in a pending state until newer PVs are made available to the cluster
	- Once new PVs are available, the PVC will automatically be bound to the newly available PV

- In the PVC definition file, the spec section includes
	- <span style="color:red">accessModes</span> = should match that on the required PV
	- <span style="color:red">resources</span> = has a section called <span style="color:red">requests</span> which has a section called <span style="color:red">storage</span> under it which holds the capacity being requested

![[pvc-4.png]]

- To view the newly created PVC, run the <span style="color:red">kubectl get persistentvolumeclaim</span> command

![[pvc-5.png]]

- When a PVC is created, Kubernetes looks at previously created PVs and binds it to one that is available

- After a PVC has been bound to a PV, run the <span style="color:red">kubectl get persistentvolumeclaim</span> command again to see that it shows the PV it has been bound to

![[pvc-6.png]]

- To delete a PVC, run the <span style="color:red">kubectl delete persistentvolumeclaim</span> command

- When a PVC is deleted, by default the underlying PV is set to be retained by setting the <span style="color:red">persistentVolumeReclaimPolicy</span> to Retain, meaning that the PV will remain until manually deleted by the admin
	- It is still not available for reuse by any other PVC

![[pvc-7.png]]

- A PV can also be chosen to be deleted automatically by setting the <span style="color:red">persistentVolumeReclaimPolicy</span> to Delete, this way as soon as the PVC is deleted, the PV is deleted as well, thus freeing up storage on the end storage device

![[pvc-8.png]]

- A third option for PVs when a PVC is deleted is to set the <span style="color:red">persistentVolumeRecolaimPolicy</span> to Recycle, meaning the data in the data volume will be scrubbed before making it available to other PVCs

![[pvc-9.png]]

- Once you create a PVC, use it in a pod definition file by specifying the PVC claim name under the "persistentVolumeClaim" section in the volumes section
	- This same is true for replicasets and deployments

![[pvc-10.png]]