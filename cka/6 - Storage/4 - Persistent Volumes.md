- Previously when we created [[3 - Volumes|volumes]], we configured them within the [[7 - Pods ✅|pod]] definition file so that all configuration information required to configure [[0 - Storage in Docker ✅|storage]] for the [[3 - Volumes|volume]] goes inside of the [[7 - Pods ✅|pod]] definition file as well

- With using just [[3 - Volumes|volumes]], when you have a large environment with a lot of <i><span style="color:#477bbe">users</span></i> [[9 - Deployments ✅|deploying]] a lot of [[7 - Pods ✅|pods]], the <i><span style="color:#477bbe">users</span></i> would have to configure [[0 - Storage in Docker ✅|storage]] every time for each [[7 - Pods ✅|pod]]
	- This means, whatever [[0 - Storage in Docker ✅|storage]] solution is used, the <i><span style="color:#477bbe">user</span></i> who [[9 - Deployments ✅|deploy]] a [[7 - Pods ✅|pod]] would have to configure that solution on all definition files in their own environment every time a change is to be made

- It is preferred that [[0 - Storage in Docker ✅|storage]] is managed centrally by an <i><span style="color:#477bbe">admin</span></i> creating a large <b><i><span style="color:#d46644">pool</span></i></b> of [[0 - Storage in Docker ✅|storage]] and the <i><span style="color:#477bbe">users</span></i> carving out the required [[0 - Storage in Docker ✅|storage]] resources from the <b><i><span style="color:#d46644">pool</span></i></b>
	- This is where <b><i><span style="color:#d46644">persistent volumes</span></i></b> can help us

![[pv-1.png]]

- A <b><i><span style="color:#d46644">persistent volume</span></i></b> (<b><i><span style="color:#d46644">PV</span></i></b>) is a [[0 - Core Concepts Intro ✅|cluster]]-wide <b><i><span style="color:#d46644">pool</span></i></b> of [[0 - Storage in Docker ✅|storage]] [[3 - Volumes|volumes]] configured by an <i><span style="color:#477bbe">administrator</span></i> to be used by <i><span style="color:#477bbe">users</span></i> [[9 - Deployments ✅|deploying]] applications on the [[0 - Core Concepts Intro ✅|cluster]]
	- <b><i><span style="color:#d46644">PVs</span></i></b> are [[3 - Volumes|volume]] plugins like [[3 - Volumes|volumes]], but have a lifecycle independent of any individual [[7 - Pods ✅|Pod]] that uses the <b><i><span style="color:#d46644">PV</span></i></b>.
	- This [[2 - Kube API server ✅|API]] object captures the details of the implementation of the [[0 - Storage in Docker ✅|storage]], be that <span style="color:#5c7e3e">NFS</span>, <span style="color:#5c7e3e">iSCSI</span>, or a cloud-provider-specific [[0 - Storage in Docker ✅|storage]] system.

- <i><span style="color:#477bbe">Users</span></i> can select [[0 - Storage in Docker ✅|storage]] from the <b><i><span style="color:#d46644">volume pool</span></i></b> using persist [[3 - Volumes|volume]] claim ([[5 - Persistent Volume Claims|PVC]])

- As with all objects in <span style="color:#5c7e3e">Kubernetes</span>, <b><i><span style="color:#d46644">persistent volumes</span></i></b> are created using definition files

![[pv-2.png]]

- Under the <span style="color:#5c7e3e">spec</span> section of the <b><i><span style="color:#d46644">persistent volume</span></i></b> definition file, specify the following:
	- <span style="color:red">accessModes</span> = defines how a [[3 - Volumes|volume]] should be mounted on the host
	- <span style="color:red">capacity</span> = specifies the amount of [[0 - Storage in Docker ✅|storage]] to be reserved for the <b><i><span style="color:#d46644">persistent volume</span></i></b>
	- [[3 - Volumes|Volume type]] = this configuration will look like the <span style="color:red">volumes</span> config that we saw in the [[7 - Pods ✅|pod]] template
		- Ie. <span style="color:red">hostPath</span> or <span style="color:red">awsElasticBlockStorage</span>

![[pv-3.png]]

- The available "<span style="color:red">accessModes</span>" for a <b><i><span style="color:#d46644">persistent volume</span></i></b> are:
	- ReadOnlyMany
	- ReadWriteOnce
	- ReadWriteMany

- After creating the <b><i><span style="color:#d46644">PV</span></i></b> using [[13 - Kubectl Apply ✅|kubectl]], use the <span style="color:red">kubectl get persistentvolume</span> command to view the newly created <b><i><span style="color:#d46644">PV</span></i></b>