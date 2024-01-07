- We create [[4 - Persistent Volumes ✅|PVs]] and [[5 - Persistent Volume Claims|PVCs]] to claim the [[4 - Persistent Volumes ✅|PV]] <b><i><span style="color:#d46644">storage</span></i></b>, and inject them into the [[7 - Pods ✅|pod]] definition files as [[3 - Volumes ✅|volumes]]

![[storage-classes-1.png]]

- In previous cases, when creating a [[5 - Persistent Volume Claims|PVC]] from a <span style="color:#5c7e3e">cloud provider</span>, the problem is that before the [[4 - Persistent Volumes ✅|PV]] is created, the <b><i><span style="color:#d46644">storage</span></i></b> had to be created on the <span style="color:#5c7e3e">cloud platform</span>
	- This means every time an application requires <b><i><span style="color:#d46644">storage</span></i></b>, you have to first manually <b><i><span style="color:#d46644">provision</span></i></b> the <b><i><span style="color:#d46644">storage</span></i></b> on the <span style="color:#5c7e3e">cloud platform</span>, manually create a [[4 - Persistent Volumes ✅|persistent volume]] definition file using the same name as that of the <b><i><span style="color:#d46644">storage</span></i></b> you created on the <span style="color:#5c7e3e">cloud platform</span>
	- This is called static <b><i><span style="color:#d46644">provisioning</span></i></b> [[3 - Volumes ✅|volumes]]

- <b><i><span style="color:#d46644">Storage classes</span></i></b> come in when [[3 - Volumes ✅|volumes]] get <b><i><span style="color:#d46644">provisioned</span></i></b> automatically when the application requires it

- With <b><i><span style="color:#d46644">storage classes</span></i></b>, you can define a <b><i><span style="color:#d46644">provisioner</span></i></b> such as <b><i><span style="color:#d46644">Google Storage</span></i></b> that can automatically <b><i><span style="color:#d46644">provision storage</span></i></b> on the <span style="color:#5c7e3e">cloud platform</span> and attach it to [[7 - Pods ✅|pods]] when a [[5 - Persistent Volume Claims|PVC]] is made

- To create a <b><i><span style="color:#d46644">storage class</span></i></b> object, the <span style="color:#5c7e3e">apiVersion</span> is storage.k8s.io/v1 and <span style="color:#5c7e3e">kind</span> is <span style="color:red">StorageClass</span>

- On the <b><i><span style="color:#d46644">storage class</span></i></b> definition file, add a <span style="color:red">provisioner</span> property with the desired provider details
	- e.g. Kubernetes.io/gce-pd

![[storage-classes-2.png]]

- Once a <b><i><span style="color:#d46644">storage class</span></i></b> is created, we no longer need the [[4 - Persistent Volumes ✅|PV]] definition as the [[4 - Persistent Volumes ✅|PV]] will be created automatically when the <b><i><span style="color:#d46644">storage class</span></i></b> is created

- For the [[5 - Persistent Volume Claims|PVC]] to use the <b><i><span style="color:#d46644">storage class</span></i></b>, specify the "<span style="color:red">storageClassName</span>" property as a child of the <span style="color:#5c7e3e">spec</span> property on the [[5 - Persistent Volume Claims|PVC]], giving the same name as that on the <b><i><span style="color:#d46644">storage class</span></i></b> definition

![[storage-classes-3.png]]

- After a <b><i><span style="color:#d46644">storage class</span></i></b> is added to a [[5 - Persistent Volume Claims|PVC]], the <b><i><span style="color:#d46644">storage class</span></i></b> uses the defined <b><i><span style="color:#d46644">provisioner</span></i></b> to <b><i><span style="color:#d46644">provision</span></i></b> new <b><i><span style="color:#d46644">storage</span></i></b> with the required size on the <span style="color:#5c7e3e">cloud platform</span>, creates a [[4 - Persistent Volumes ✅|PV]], then [[3 - Volumes ✅|binds]] the [[5 - Persistent Volume Claims|PVC]] to that [[4 - Persistent Volumes ✅|PV]]

![[storage-classes-4.png]]

- NOTE: with <b><i><span style="color:#d46644">storage classes</span></i></b>, [[4 - Persistent Volumes ✅|PVs]] are still needed, you just don't have to manually create them

- There are many other <b><i><span style="color:#d46644">provisioners</span></i></b> for <b><i><span style="color:#d46644">storage classes</span></i></b>

![[storage-classes-5.png]]

- With each <b><i><span style="color:#d46644">storage class</span></i></b> <b><i><span style="color:#d46644">provisioner</span></i></b>, you can pass in additional parameters which are specific to the <b><i><span style="color:#d46644">provisioner</span></i></b> being used
	- e.g. the type of disk to <b><i><span style="color:#d46644">provision</span></i></b>, the replication type, etc…

![[storage-classes-6.png]]

- You can create different <b><i><span style="color:#d46644">storage classes</span></i></b> with different types of disk
	- This is why it is called <b><i><span style="color:#d46644">storage classes</span></i></b>, because you create different <span style="color:red">classes</span> of <b><i><span style="color:#d46644">storage</span></i></b>

![[storage-classes-7.png]]

- The <span style="color:red">volumeBinderMode</span> property being set to <span style="color:red">waitForFirstConsumer</span> will delay the [[3 - Volumes ✅|binding]] and <b><i><span style="color:#d46644">provisioning</span></i></b> of a [[4 - Persistent Volumes ✅|PV]] until a [[7 - Pods ✅|pod]] using the [[5 - Persistent Volume Claims|PVC]] is created