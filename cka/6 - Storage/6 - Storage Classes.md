- We create PVs and PVCs to claim the PV storage, and inject them into the pod definition files as volumes

![[storage-classes-1.png]]

- In previous cases, when creating a PVC from a cloud provider, the problem is that before the PV is created, the storage had to be created on the cloud platform
	- This means every time an application requires storage, you have to first manually provision the storage on the cloud platform, manually create a persistent volume definition file using the same name as that of the storage you created on the cloud platform
	- This is called static provisioning volumes

- Storage classes come in when volumes get provisioned automatically when the application requires it

- With storage classes, you can define a provisioner such as Google Storage that can automatically provision storage on the cloud platform and attach it to pods when a PVC is made

- To create a storage class object, the apiVersion is storage.k8s.io/v1 and kind is <span style="color:red">StorageClass</span>

- On the storage class definition file, add a <span style="color:red">provisioner</span> property with the desired provider details
	- e.g. Kubernetes.io/gce-pd

![[storage-classes-2.png]]

- Once a storage class is created, we no longer need the PV definition as the PV will be created automatically when the storage class is created

- For the PVC to use the storage class, specify the "storageClassName" property as a child of the spec property on the PVC, giving the same name as that on the storage class definition

![[storage-classes-3.png]]

- After a storage class is added to a PVC, the storage class uses the defined provisioner to provision new storage with the required size on the cloud platform, creates a PV, then binds the PVC to that PV

![[storage-classes-4.png]]

- NOTE: with storage classes, PVs are still needed, you just don't have to manually create them

- There are many other provisioners for storage classes

![[storage-classes-5.png]]

- With each storage class provisioner, you can pass in additional parameters which are specific to the provisioner being used
	- e.g. the type of disk to provision, the replication type, etc…

![[storage-classes-6.png]]

- You can create different storage classes with different types of disk
	- This is why it is called storage classes, because you create different <span style="color:red">classes</span> of storage

![[storage-classes-7.png]]

- The <span style="color:red">volumeBinderMode</span> property being set to <span style="color:red">waitForFirstConsumer</span> will delay the binding and provisioning of a PV until a pod using the PVC is created