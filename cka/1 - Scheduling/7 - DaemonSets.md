- <b><span style="color:#d46644">DaemonSets</span></b>, like [[8 - ReplicaSets ✅|ReplicaSets]], help you deploy multiple instances of a [[7 - Pods ✅|pod]]
	- It runs only <u>one</u> copy of the [[7 - Pods ✅|pod]] on each [[0 - Core Concepts Intro ✅|node]] in the [[0 - Core Concepts Intro ✅|cluster]]

- With <b><span style="color:#d46644">DaemonSets</span></b>, whenever a new [[0 - Core Concepts Intro ✅|node]] is added to the [[0 - Core Concepts Intro ✅|cluster]], a replica of that [[7 - Pods ✅|pod]] is automatically added to that [[0 - Core Concepts Intro ✅|node]]

![[daemonsets-1.png]]

- With <b><span style="color:#d46644">DaemonSets</span></b>, when a [[0 - Core Concepts Intro ✅|node]] is removed, the [[7 - Pods ✅|pod]] is also automatically removed

![[daemonsets-2.png]]

- The <b><span style="color:#d46644">DaemonSet</span></b> ensures that one copy of the [[7 - Pods ✅|pod]] is always present on every [[0 - Core Concepts Intro ✅|node]] in the [[0 - Core Concepts Intro ✅|cluster]]

### Use-Cases of DaemonSets

- To deploy a monitoring agent or log collector on each [[0 - Core Concepts Intro ✅|node]] in the [[0 - Core Concepts Intro ✅|cluster]]
	- This way you never have to worry about adding or removing monitoring and logging from the [[0 - Core Concepts Intro ✅|nodes]] when there are changes in the [[0 - Core Concepts Intro ✅|cluster]] as the <b><span style="color:#d46644">DaemonSet</span></b> will automatically always keep them active

- Since we know that [[0 - Core Concepts Intro ✅|kube-proxy]] is required on every [[0 - Core Concepts Intro ✅|node]] in the [[0 - Core Concepts Intro ✅|cluster]], it is a good use-case for a <b><span style="color:#d46644">DaemonSet</span></b>

- Also, since <i><span style="color:#477bbe">networking</span></i> (ie. Flannel) needs to be on every [[0 - Core Concepts Intro ✅|node]] so that they are able to communicate with each other and every [[7 - Pods ✅|pod]], it is also a good use-case for a <b><span style="color:#d46644">DaemonSet</span></b>

- The <b><span style="color:#d46644">DaemonSet</span></b> creation process is similar to the [[8 - ReplicaSets ✅|replicaSet]] creation process
	- The difference is the <i><span style="color:#477bbe">kind</span></i>

- The <b><span style="color:#d46644">DaemonSet</span></b> definition file has a nested [[7 - Pods ✅|pod]] <i><span style="color:#477bbe">spec</span></i> under the template section and [[1 - Labels & Selectors|selectors]] to link the <b><span style="color:#d46644">DaemonSet</span></b> to the [[7 - Pods ✅|pods]]

![[daemonsets-3.png]]

- To create a <b><span style="color:#d46644">DaemonSet</span></b> <i><span style="color:#477bbe">imperatively</span></i>, use the <span style="color:red">kubectl create deployment</span> command and change the <i><span style="color:#477bbe">kind</span></i> to <b><span style="color:#d46644">DaemonSet</span></b> as there is no ~~kubectl create daemonset~~ command
	- Remove the replica, strategy and <i><span style="color:#477bbe">status</span></i> fields

- To view a <b><span style="color:#d46644">DaemonSet</span></b> , use the <span style="color:red">kubectl get daemonsets</span>

- To view more details about the <b><span style="color:#d46644">DaemonSet</span></b>, use the <span style="color:red">kubectl describe daemonset</span> command

### How DaemonSets Work

- One way is by setting the nodeName property on each [[7 - Pods ✅|pod]] <i><span style="color:#477bbe">spec</span></i> before the [[7 - Pods ✅|pod]] is created
	- This is the way scheduling a [[7 - Pods ✅|pod]] on each [[0 - Core Concepts Intro ✅|node]] used to work

- From Kubernetes v1.12 onwards, the <b><span style="color:#d46644">DaemonSet</span></b> uses the default [[4 - Kube Scheduler ✅|scheduler]] and [[4 - Node Affinity|nodeAffinity]] rules to schedule [[7 - Pods ✅|pods]] on [[0 - Core Concepts Intro ✅|nodes]]
