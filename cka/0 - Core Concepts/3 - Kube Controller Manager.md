- The <b><span style="color:#d46644">kube-controller-manager</span></b> manages various <i><span style="color:#d46644">controllers</span></i> in <span style="color:#5c7e3e">Kubernetes</span>

- A <i><span style="color:#d46644">controller</span></i> is a process that continuously monitors the state of various components in the system and works towards bringing the system from its current state to the desired functioning state

- The <span style="color:#5c7e3e">Kubernetes</span> <b><span style="color:#d46644">controller-manager</span></b>  is a daemon (OS service) that embeds the core <span style="color:#5c7e3e">control loops</span> shipped with <span style="color:#5c7e3e">Kubernetes</span>.
	- A <span style="color:#5c7e3e">control loops</span> is a non-terminating loop that regulates the state of the system.

### NODE CONTROLLER

- The <i><span style="color:#d46644">node-controller</span></i> is responsible for monitoring the status of the node and takes the necessary actions to keep the application running
	- It does this through the [[2 - Kube API server|kube-apiserver]]

- The <i><span style="color:#d46644">node-controller</span></i> checks the status of the [[0 - Core Concepts Intro|node]] every <b><span style="color:#5c7e3e">5 seconds</span></b>
	- If the <i><span style="color:#d46644">controller</span></i> stops receiving updates from a [[0 - Core Concepts Intro|node]], the =[[0 - Core Concepts Intro|nodes]]= is marked as unreachable
		- The <i><span style="color:#d46644">controller</span></i> waits <b><span style="color:#5c7e3e">40 seconds</span></b> before marking a [[0 - Core Concepts Intro|node]] as unreachable

- After a [[0 - Core Concepts Intro|node]] is marked unreachable, the <i><span style="color:#d46644">controller</span></i> gives it <b><span style="color:#5c7e3e">5 minutes</span></b> to come back up
	- If the [[0 - Core Concepts Intro|node]] doesn't come back up, it terminates that [[0 - Core Concepts Intro|node]] and assigns the [[7 - Pods|pods]] on the [[0 - Core Concepts Intro|node]] to healthy running [[0 - Core Concepts Intro|nodes]] (if the [[7 - Pods|pods]] are part of a [[8 - ReplicaSets|replicaSets]])

### REPLICATION CONTROLLER

- The <i><span style="color:#d46644">replication-controller</span></i> is responsible for monitoring the status of [[8 - ReplicaSets|replicaSets]], ensuring that the desired number of [[7 - Pods|pods]] are available at all times within the set
	- If a [[7 - Pods|pod]] dies, it creates another one

#### OTHER TYPES OF CONTROLLERS
- Deployment controller
- Cronjob
- Service-Account controller
- Namespace controller
- Job controller
- Stateful set
- PV-Binder controller
- Endpoint controller
- PV-Protection controller
- Replicaset

- The various <i><span style="color:#d46644">controllers</span></i> are all packaged into a single process known as the <span style="color:#5c7e3e">Kubernetes</span> <b><span style="color:#d46644">controller-manager</span></b> 

- When you install the <b><span style="color:#d46644">controller-manager</span></b> , the different <i><span style="color:#d46644">controllers</span></i> automatically get installed also

- To install the <b><span style="color:#d46644">controller-manager</span></b>, you would download the <b><span style="color:#d46644">kube-controller-manager</span></b> from the <span style="color:#5c7e3e">Kubernetes</span> release page, extract it, and run it as a service
	- When you run the service, you see additional options for configuring the <i><span style="color:#d46644">controllers</span></i>

- By default, all <i><span style="color:#d46644">controllers</span></i> are <span style="color:#5c7e3e">enabled</span>, but you can specify which to enable and disable by using the <span style="color:red">--controllers</span> option

- You can see the options in the <b><span style="color:#d46644">controller-manager</span></b>  manifest by navigating to the <span style="color:red">/etc/kubernetes/manifests directory</span>

	`cat /etc/kubernetes/manifests/kube-controller-manager.yaml`

- You can inspect the <i><span style="color:#d46644">controller</span></i> by viewing the <b><span style="color:#d46644">kube-controller-manager</span></b> service

	`cat /etc/systemd/system/kube-controller-manager.service`