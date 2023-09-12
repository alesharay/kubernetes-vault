- The <b><span style="color:#d46644">master node</span></b> is responsible for maintaining the <b><span style="color:#5c7e3e">Kubernetes</span></b> <b><span style="color:#d46644">cluster</span></b>
	- The <b><span style="color:#d46644">master node</span></b> completes this task by using a set of components called the <b><span style="color:#d46644">control plane</span></b> components

- The <b><span style="color:#d46644">worker node</span></b> is responsible for carrying out all of the tasks communicated by the <b><span style="color:#d46644">master node</span></b> and hosting the applications as <b><span style="color:#d46644">containers</span></b>

- The <b><span style="color:#d46644">control plane</span></b> components include:
	- <b><span style="color:#d46644">etcd</span></b>:
		- a key-value store/database that maintains information about the <b><span style="color:#d46644">nodes</span></b>, what [[7 - Pods|pods]] are on which <b><span style="color:#d46644">nodes</span></b>, the times the events took place, etc…

	- <b><span style="color:#d46644">scheduler</span></b>:
		- Identifies the right <b><span style="color:#d46644">node</span></b> to place a [[7 - Pods|container / pod]]on based on the <b><span style="color:#d46644">container's</span></b> resource requirements, the <b><span style="color:#d46644">node's</span></b> capacity, and other policies and constraints

	- <b><span style="color:#d46644">controllers</span></b>:
		- Takes care of different areas
			- Controller manager
				- [[3 - Kube Controller Manager|Node Controller]]
					- Takes care of <b><span style="color:#d46644">nodes</span></b>
					- Responsible for onboarding new <b><span style="color:#d46644">nodes</span></b> to the <b><span style="color:#d46644">cluster</span></b>, handling situations where <b><span style="color:#d46644">nodes</span></b> become unavailable
				- [[3 - Kube Controller Manager|Replication Controller]]
					- Ensures that the desired number of <b><span style="color:#d46644">containers</span></b> is running at all times

	- <b><span style="color:#d46644">kube-apiserver</span></b>:
		- This is the primary management component of <b><span style="color:#5c7e3e">Kubernetes</span></b>
		- Responsible for orchestrating all operations within the <b><span style="color:#d46644">cluster</span></b>
		- Exposes the <b><span style="color:#5c7e3e">Kubernetes</span></b> API, which is used by external users to perform management operations on the <b><span style="color:#d46644">cluster</span></b>

	- <b><span style="color:#d46644">container runtime engine</span></b>:
		- Everything in <b><span style="color:#5c7e3e">Kubernetes</span></b> runs on <b><span style="color:#d46644">containers</span></b> (as it is a <b><span style="color:#d46644">container</span></b> orchestration tool) and as such, needs the software that can run the <b><span style="color:#d46644">containers</span></b>
		- This engine (ie. Docker, containerd, rkt) should be installed on all of the <b><span style="color:#d46644">nodes</span></b> in the <b><span style="color:#d46644">cluster</span></b>

	- <b><span style="color:#d46644">kubelet</span></b>:
		- Engine that runs on each <b><span style="color:#d46644">node</span></b>, listening to instructions from the [[2 - Kube API server ✅|apiserver]]
		- Manages all activities on the node
		- Communicates with the <b><span style="color:#d46644">master node</span></b> both sending and receiving relevant information about the <b><span style="color:#d46644">node</span></b>

	- <b><span style="color:#d46644">kube-proxy</span></b>:
		- Ensures that the necessary rules are in place on the <b><span style="color:#d46644">worker node</span></b> to allow the <b><span style="color:#d46644">containers</span></b> running on them to reach each other