- <b><span style="color:#d46644">Etcd</span></b> is a distributed, reliable key-value store that is Simple, Secure & Fast

- A <span style="color:#d46644">key-value store</span>saves information in key-value pair format
	- <b><i><span style="color:#5c7e3e">Put</span></i></b> (key, value) - places the key and value into the data
	- <b><i><span style="color:#5c7e3e">Get</span></i></b> (key) - returns the value

### **Install ETCD**

1. <b><i><span style="color:#5c7e3e">Download Binaries</span></i></b> specific to your OS which will give a zipped/compressed file
2. <b><i><span style="color:#5c7e3e">Extract</span></i></b> the binaries and give them executable permissions
3. <b><i><span style="color:#5c7e3e">Run ETCD Service</span></i></b>
	1. Run the binaries
	2. Listens on port <span style="color:#5c7e3e">2379</span>

- A default client that comes with <b><span style="color:#d46644">etcd</span></b> is the <b><span style="color:#d46644">etcd control client (etcdctl)</span></b>
	- This can be used to store/retrieve key value pairs
		- To store a key-value pair, use the set command
		- To retrieve a key-value pair, use the get command
		- Running the <b><span style="color:#d46644">etcdctl</span></b> command on its own gives the help menu

### **ETCD in Kubernetes**

- The <b><span style="color:#d46644">etcd datastore</span></b> stores information regarding the [[0 - Core Concepts Intro|cluster]], such as the [[0 - Core Concepts Intro|node]], [[7 - Pods|pods]], <i><span style="color:#477bbe">configs</span></i>, <i><span style="color:#477bbe">secrets</span></i>, <i><span style="color:#477bbe">accounts</span></i>, <i><span style="color:#477bbe">roles</span></i>, <i><span style="color:#477bbe">bindings</span></i>, other

- All of the information we see when we run the `kubectl get` command comes from <b><span style="color:#d46644">etcd</span></b>

- Every change you make to the [[0 - Core Concepts Intro|cluster]] such as adding additional [[0 - Core Concepts Intro|nodes]], deploying [[7 - Pods|pods]] or [[8 - ReplicaSets|replicaSets]] are updated in the <b><span style="color:#d46644">etcd server</span></b>

- Depending on how you setup your [[0 - Core Concepts Intro|cluster]], <b><span style="color:#d46644">etcd</span></b> can be deployed differently
	- If a [[0 - Core Concepts Intro|cluster]] is setup from scratch, you download <b><span style="color:#d46644">etcd</span></b> binaries manually and configure <b><span style="color:#d46644">etcd</span></b> as a service in the [[0 - Core Concepts Intro|master node]]
	- If a [[0 - Core Concepts Intro|cluster]] is setup using <b><i><span style="color:#5c7e3e">kubeadm</span></i></b>, the <b><span style="color:#d46644">etcd server</span></b> is setup for you as a [[7 - Pods|pod]] in the kube-system [[11 - Namespaces|namespace]]

- You can explore the <b><span style="color:#d46644">etcd database</span></b> by running the <span style="color:red">kubectl exec</span> command inside of the [[0 - Core Concepts Intro|master node]]

<span style="color:red">kubectl get pods -n kube-system</span> (this gives the name of the <b><span style="color:#d46644">etcd pod</span></b>)

`kubectl exec etcd-NAME_OF_POD -n kube-system etcdctl get / --prefix --keys-only`

- When <b><span style="color:#d46644">etcd</span></b>  is running in a [[7 - Pods|pod]] on the [[0 - Core Concepts Intro|cluster]], this means that <b><span style="color:#d46644">etcd</span></b> is set up as a <b><span style="color:#5c7e3e">Stacked ETCD Topology</span></b> where the distributed data storage [[0 - Core Concepts Intro|cluster]] provided by <b><span style="color:#d46644">etcd</span></b> is <b><span style="color:#5c7e3e">stacked</span></b> on top of the [[0 - Core Concepts Intro|cluster]] formed by the [[0 - Core Concepts Intro|nodes]] managed by <i><span style="color:#477bbe">kubeadm</span></i> that runs <i><span style="color:#477bbe">controlplane</span></i> components

- If there is no <b><span style="color:#d46644">etcd</span></b> [[7 - Pods|pod]] and no <b><span style="color:#d46644">etcd</span></b> [[7 - Pods|pod]] manifest file, <b><i><span style="color:#5c7e3e">ssh</span></i></b> into the [[0 - Core Concepts Intro|node]] where you are searching for <b><span style="color:#d46644">etcd</span></b>
	- Run the <span style="color:red">ps ef | grep -I etcd</span> command to find that the [[2 - Kube API server|kube-apiserver]] is, in fact, referencing an <b><span style="color:#d46644">etcd store</span></b>
	- This can also be seen by describing the [[2 - Kube API server|kube-apiserver]] [[7 - Pods|pod]]
	- This means that the <b><span style="color:#d46644">etcd</span></b> <i><span style="color:#477bbe">datastore</span></i> is <b><i><span style="color:#5c7e3e">external</span></i></b>

![[etcd-1.png]]

* To access an external <b><span style="color:#d46644">etcd datastore</span></b>, <b><i><span style="color:#5c7e3e">ssh</span></i></b> into either the <i><span style="color:#477bbe">IP address</span></i> or the <i><span style="color:#477bbe">hostname</span></i> of the <b><span style="color:#d46644">etcd server</span></b>

- To check how many members ([[0 - Core Concepts Intro|nodes]]) are part of an external <b><span style="color:#d46644">etcd server</span></b>, run the <span style="color:red">etcdctl members list</span> command

![[etcd-2.png]]



## **ETCD Optional Commands**

ETCD – Commands (Optional)

(Optional) Additional information about ETCDCTL Utility
	- ETCDCTL is the CLI tool used to interact with ETCD.
	- ETCDCTL can interact with ETCD Server using 2 API versions – Version 2 and Version 3. 
		- By default it’s set to use Version 2.
	- Each version has different sets of commands.

For example, ETCDCTL version 2 supports the following commands:

		etcdctl backup
		etcdctl cluster-health
		etcdctl mk
		etcdctl mkdir
		etcdctl set

Whereas the commands are different in version 3

		etcdctl snapshot save
		etcdctl endpoint health
		etcdctl get
		etcdctl put

To set the right version of the API, set the environment variable ETCDCTL_API

		export ETCDCTL_API=3

	- When the API version is not set, it is assumed to be set to version 2. And version 3 commands listed above don’t work.
	- When API version is set to version 3, version 2 commands listed above don’t work.
	- Apart from that, you must also specify the path to certificate files so that ETCDCTL can authenticate to the ETCD API Server.
	- The certificate files are available in the etcd-master at the following path.

		--cacert /etc/kubernetes/pki/etcd/ca.crt
		--cert /etc/kubernetes/pki/etcd/server.crt
		--key /etc/kubernetes/pki/etcd/server.key

	- You must specify the ETCDCTL_API version and path to certificate files.
	- Below is the final form:

		kubectl exec etcd-controlplane -n kube-system -- sh -c \ "ETCDCTL_API=3 etcdctl get /
		--prefix --keys-only --limit=10
		--cacert /etc/kubernetes/pki/etcd/ca.crt
		--cert /etc/kubernetes/pki/etcd/server.crt
		--key /etc/kubernetes/pki/etcd/server.key"
