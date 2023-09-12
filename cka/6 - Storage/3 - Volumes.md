- <span style="color:#5c7e3e">Docker</span> [[7 - Pods|containers]] are meant to be **transient** in nature
	- This means they are meant to last only for a short period of time

- To [[4 - Persistent Volumes|persist]] data processed by the [[7 - Pods|container]], we attach a <b><i><span style="color:#d46644">volume</span></i></b> to the [[7 - Pods|container]] when it is created

- When <b><i><span style="color:#d46644">volumes</span></i></b> are created, the data processed by the [[7 - Pods|container]] is now placed in the <b><i><span style="color:#d46644">volume</span></i></b> thereby retaining it permanently

- Just as with <span style="color:#5c7e3e">Docker</span>, the [[7 - Pods|pods]] created in <span style="color:#5c7e3e">Kubernetes</span> are **transient** in nature

- To [[4 - Persistent Volumes|persist]] data in <span style="color:#5c7e3e">Kubernetes</span>, we attach a <b><i><span style="color:#d46644">volume</span></i></b> to the [[7 - Pods|pod]]
	- Therefore, even after the [[7 - Pods|pod]] is deleted, the data remains

- To create a <b><i><span style="color:#d46644">volume</span></i></b> on a [[7 - Pods|pod]], add a <b><i><span style="color:#d46644">volumes</span></i></b> property to the [[7 - Pods|pod]] definition file with the name of the <b><i><span style="color:#d46644">volume</span></i></b> added to the <span style="color:red">name</span> property under it
	- The <b><i><span style="color:#d46644">volumes</span></i></b> property is a sibling of the [[7 - Pods|containers]] property in the [[7 - Pods|pod]] spec

![[volumes-1.png]]

- Because a <b><i><span style="color:#d46644">volume</span></i></b> needs a [[0 - Storage in Docker|storage]], when creating a <b><i><span style="color:#d46644">volume</span></i></b>, you can choose to configure its [[0 - Storage in Docker|storage]] in different ways

- If using a directory on the host as the [[0 - Storage in Docker|storage]] location for the <b><i><span style="color:#d46644">volume</span></i></b>, add the <span style="color:red">hostPath</span> property under the <b><i><span style="color:#d46644">volumes</span></i></b> property on the [[7 - Pods|pod]] spec
	- Under this, we add the <span style="color:red">path</span> and <span style="color:red">type</span> properties with the location of the path and type of the path respectively

![[volumes-2.png]]

- Once a <b><i><span style="color:#d46644">volume</span></i></b> is created, to access it from a [[7 - Pods|container]], we <b><i><span style="color:#d46644">mount</span></i></b> the <b><i><span style="color:#d46644">volume</span></i></b> to a directory inside of the [[7 - Pods|container]]

- To <b><i><span style="color:#d46644">mount</span></i></b> a <b><i><span style="color:#d46644">volume</span></i></b> to a directory inside of the [[7 - Pods|container]], we add the <span style="color:red">volumeMounts</span> property as a child of the [[7 - Pods|containers]] property in the [[7 - Pods|pod]] spec
	- Under this, we add the <span style="color:red">mountPath</span> and <span style="color:red">name</span> properties with the location of the path inside of the [[7 - Pods|container]] and the name of the <b><i><span style="color:#d46644">volume</span></i></b> respectively

![[volumes-3.png]]

- The <span style="color:red">hostPath</span> property is fine if the [[0 - Core Concepts Intro ✅|cluster]] is made up of a [[0 - Core Concepts Intro ✅|single node]] but is not recommended if using a [[0 - Core Concepts Intro ✅|multi-node cluster]]
	- This is because the [[7 - Pods|pods]] will use that directory on all [[0 - Core Concepts Intro ✅|nodes]] and expect all of them to be the same and have the same data
	- Since they are on different <i><span style="color:#477bbe">servers</span></i> they are, in fact, not the same; unless you configure some sort of external replicated [[0 - Core Concepts Intro ✅|cluster]] [[0 - Storage in Docker|storage]] solution

- There are <b><i><span style="color:#d46644">volume storage</span></i></b> solutions for the [[7 - Pods|pod]] spec in addition to the <span style="color:red">hostPath</span> property that can be configured
	- NFS
	- GlusterFS
	- Flocker
	- FiberChannel
	- CephFS
	- ScaleIO
	- AWS EBS (public cloud solution)
	- Azure Disk or File (public cloud solution)
	- Google Persistent Disk (public cloud solution)

- Each [[0 - Storage in Docker|storage]] solution has a different configuration property for the [[7 - Pods|pod]] spec
	- Ie. <span style="color:#5c7e3e">AWS EBS</span> uses the "<span style="color:red">awsElasticBlockStorage</span>" property with the "<span style="color:red">volumeID</span>" and "<span style="color:red">fsType</span>" properties under it

![[volumes-4.png]]