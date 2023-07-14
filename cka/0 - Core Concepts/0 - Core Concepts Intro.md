- The <b><span style="color:#d46644">master node</span></b> is responsible for maintaining the <b><span style="color:#5c7e3e">Kubernetes</span></b> <b><span style="color:#d46644">cluster</span></b>
	- The <b><span style="color:#d46644">master node</span></b> completes this task by using a set of components called the <b><span style="color:#d46644">control plane</span></b> components

- The <b><span style="color:#d46644">worker node</span></b> is responsible for carrying out all of the tasks communicated by the <b><span style="color:#d46644">master node</span></b> and hosting the application as <i><span style="color:#477bbe">containers</span></i>

- The <b><span style="color:#d46644">control plane</span></b> components include:
	- <b><span style="color:#d46644">etcd</span></b>:
		- a key-value store/database that maintains information about the <b><span style="color:#d46644">nodes</span></b>, what [[7 - Pods|pods]] are on which <b><span style="color:#d46644">nodes</span></b>, the times the events took place, etc…

	- <b><span style="color:#d46644">scheduler</span></b>:
		- Identifies the right <b><span style="color:#d46644">node</span></b> to place a [[7 - Pods|container / pod]]on based on the <i><span style="color:#477bbe">container's</span></i> resource requirements, the <b><span style="color:#d46644">node's</span></b> capacity, and other policies and constraints

	- <b><span style="color:#d46644">controllers</span></b>:
		- Takes care of different areas
			- Controller manager
				- <i><span style="color:#477bbe">Node controller</span></i>
					- Takes care of nodes
					- Responsible for onboarding new nodes to the <b><span style="color:#d46644">cluster</span></b>, handling situations where nodes become unavailable
				- <i><span style="color:#477bbe">Replication Controller</span></i>
					- Ensures that the desired number of <i><span style="color:#477bbe">containers</span></i> is running at all times

	- <b><span style="color:#d46644">kube-apiserver</span></b>:
		- This is the primary management component of k8s
		- Responsible for orchestrating all operations within the <i><span style="color:#477bbe">cluster</span></i>
		- Exposes the k8s API, which is used by external users to perform management operations on the <i><span style="color:#477bbe">cluster</span></i>

	- <b><span style="color:#d46644">container runtime engine</span></b>:
		- Everything in k8s runs on <i><span style="color:#477bbe">containers</span></i> (as it is a <i><span style="color:#477bbe">container</span></i> orchestration tool) and as such, needs the software that can run the <i><span style="color:#477bbe">containers</span></i>
		- This engine (ie. Docker, containerd, rkt) should be installed on all of the <b><span style="color:#d46644">nodes</span></b> in the <i><span style="color:#477bbe">cluster</span></i>

	- <b><span style="color:#d46644">kubelet</span></b>:
		- Engine that runs on each <b><span style="color:#d46644">node</span></b>, listening to instructions from the [[2 - Kube API server|apiserver]]
		- Manages all activities on the node
		- Communicates with the <b><span style="color:#d46644">master node</span></b> both sending and receiving relevant information about the <b><span style="color:#d46644">node</span></b>

	- <b><span style="color:#d46644">kube-proxy</span></b>:
		- Ensures that the necessary rules are in place on the <b><span style="color:#d46644">worker node</span></b> to allow the <i><span style="color:#477bbe">containers</span></i> running on them to reach each other