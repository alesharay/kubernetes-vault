- [[4 - Persistent Volumes|Persistent volumes]] and <b><i><span style="color:#d46644">persistent volume claims</span></i></b> are two separate objects in the <span style="color:#5c7e3e">Kubernetes</span> [[11 - Namespaces|namespace]]

- <i><span style="color:#477bbe">Administrators</span></i> create [[4 - Persistent Volumes|persistent volumes]] 

- <i><span style="color:#477bbe">Users</span></i> create <b><i><span style="color:#d46644">persistent volume claims</span></i></b>

- Once <b><i><span style="color:#d46644">PVCs</span></i></b> are created, <span style="color:#5c7e3e">Kubernetes</span> [[3 - Volumes|bind]] [[4 - Persistent Volumes|PVs]] to <b><i><span style="color:#d46644">PVCs</span></i></b> based on requests and properties set on the [[3 - Volumes|volume]]

- Every <b><i><span style="color:#d46644">PVC</span></i></b> is bound to a single [[4 - Persistent Volumes|PV]]

![[pvc-1.png]]

- During the [[3 - Volumes|binding]] process, <span style="color:#5c7e3e">Kubernetes</span> tries to find a [[4 - Persistent Volumes|PV]] with the sufficient capacity requested by the <b><i><span style="color:#d46644">PVC</span></i></b> along with any other request properties such as accessModes, [[3 - Volumes|volume]] modes, [[6 - Storage Classes|storage classes]], etc…

- If there are multiple possible [[4 - Persistent Volumes|PV]] matches for a single <b><i><span style="color:#d46644">PVC</span></i></b>, and you would like to specifically use a particular [[3 - Volumes|volume]], you can use [[1 - Labels & Selectors|labels and selectors]] to [[3 - Volumes|bind]] to the right [[3 - Volumes|volumes]]

![[pvc-2.png]]

- NOTE: a smaller <b><i><span style="color:#d46644">PVC</span></i></b> may get bound to a larger [[4 - Persistent Volumes|PV]] if all other criteria matches and there are no better options

- There is a one-to-one relationship between <b><i><span style="color:#d46644">PVCs</span></i></b> and [[4 - Persistent Volumes|PVs]], thus no other <b><i><span style="color:#d46644">PVC</span></i></b> can utilize the remaining capacity in the [[4 - Persistent Volumes|PV]]

![[pvc-3.png]]

- If there are no [[4 - Persistent Volumes|PVs]] available, the <b><i><span style="color:#d46644">PVC</span></i></b> will remain in a pending state until newer [[4 - Persistent Volumes|PVs]] are made available to the [[0 - Core Concepts Intro ✅|cluster]]
	- Once new [[4 - Persistent Volumes|PVs]] are available, the <b><i><span style="color:#d46644">PVC</span></i></b> will automatically be [[3 - Volumes|bound]] to the newly available [[4 - Persistent Volumes|PV]]

- In the <b><i><span style="color:#d46644">PVC</span></i></b> definition file, the <span style="color:#5c7e3e">spec</span> section includes
	- <span style="color:red">accessModes</span> = should match that on the required [[4 - Persistent Volumes|PV]]
	- <span style="color:red">resources</span> = has a section called <span style="color:red">requests</span> which has a section called <span style="color:red">storage</span> under it which holds the capacity being requested

![[pvc-4.png]]

- To view the newly created <b><i><span style="color:#d46644">PVC</span></i></b>, run the <span style="color:red">kubectl get persistentvolumeclaim</span> command

![[pvc-5.png]]

- When a <b><i><span style="color:#d46644">PVC</span></i></b> is created, <span style="color:#5c7e3e">Kubernetes</span> looks at previously created [[4 - Persistent Volumes|PVs]] and [[3 - Volumes|binds]] it to one that is available

- After a <b><i><span style="color:#d46644">PVC</span></i></b> has been [[3 - Volumes|bound]] to a [[4 - Persistent Volumes|PV]], run the <span style="color:red">kubectl get persistentvolumeclaim</span> command again to see that it shows the [[4 - Persistent Volumes|PV]] it has been [[3 - Volumes|bound]] to

![[pvc-6.png]]

- To delete a <b><i><span style="color:#d46644">PVC</span></i></b>, run the <span style="color:red">kubectl delete persistentvolumeclaim</span> command

- When a <b><i><span style="color:#d46644">PVC</span></i></b> is deleted, by default the underlying [[4 - Persistent Volumes|PV]] is set to be retained by setting the <span style="color:red">persistentVolumeReclaimPolicy</span> to Retain, meaning that the [[4 - Persistent Volumes|PV]] will remain until manually deleted by the <i><span style="color:#477bbe">admin</span></i>
	- It is still not available for reuse by any other <b><i><span style="color:#d46644">PVC</span></i></b>

![[pvc-7.png]]

- A [[4 - Persistent Volumes|PV]] can also be chosen to be deleted automatically by setting the <span style="color:red">persistentVolumeReclaimPolicy</span> to <span style="color:red">Delete</span>, this way as soon as the <b><i><span style="color:#d46644">PVC</span></i></b> is deleted, the [[4 - Persistent Volumes|PV]] is deleted as well, thus freeing up [[0 - Storage in Docker|storage]] on the end [[0 - Storage in Docker|storage devices]]

![[pvc-8.png]]

- A third option for [[4 - Persistent Volumes|PVs]] when a <b><i><span style="color:#d46644">PVC</span></i></b> is deleted is to set the <span style="color:red">persistentVolumeReclaimPolicy</span> to <span style="color:red">Recycle</span>, meaning the data in the data [[3 - Volumes|volume]] will be scrubbed before making it available to other <b><i><span style="color:#d46644">PVCs</span></i></b>

![[pvc-9.png]]

- Once you create a <b><i><span style="color:#d46644">PVC</span></i></b>, use it in a [[7 - Pods|pod]] definition file by specifying the <b><i><span style="color:#d46644">PVC</span></i></b> name under the "<span style="color:red">persistentVolumeClaim</span>" section in the [[3 - Volumes|volumes]] section
	- This same is true for [[8 - ReplicaSets|replicaSets]] and [[9 - Deployments|deployments]]

![[pvc-10.png]]