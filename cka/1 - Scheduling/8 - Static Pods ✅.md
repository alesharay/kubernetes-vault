#flashcards/kubernetes/cka/scheduling

- [[5 - Kubelet ✅|Kubelet]] can manage [[0 - Core Concepts Intro ✅|nodes]] independently

- If [[5 - Kubelet ✅|kubelet]] has no [[2 - Kube API server ✅|kube-apiserver]] or [[1 - ETCD ✅|etcd]] store to communicate with, there are still things that can be done in order to deploy [[7 - Pods ✅|pods]] onto [[0 - Core Concepts Intro ✅|nodes]]

- [[7 - Pods ✅|pod]] definition files can be placed in a directory on the server and [[5 - Kubelet ✅|kubelet]] can read those files to create the [[7 - Pods ✅|pods]]

- [[5 - Kubelet ✅|Kubelet]] periodically checks the <i><span style="color:#5c7e3e">manifest</span></i> directory for files, reads those files to create [[7 - Pods ✅|pods]], and makes sure they stay alive

- If the application crashes, [[5 - Kubelet ✅|kubelet]] attempts to restart it

- If changes are made to any of the files in the <i><span style="color:#5c7e3e">manifest</span></i> directory, [[5 - Kubelet ✅|kubelet]] recreates the [[7 - Pods ✅|pods]] for those changes to take effect

- If a file is deleted from the <i><span style="color:#5c7e3e">manifest</span></i> directory, that [[7 - Pods ✅|pod]] is deleted automatically

- These [[7 - Pods ✅|pods]] that are created directly from <i><span style="color:#5c7e3e">manifest</span></i> files without input from any of the [[0 - Core Concepts Intro ✅|control plane]] components ([[2 - Kube API server ✅|kube-apiserver]], [[1 - ETCD ✅|etcd]], etc…) are known as <b><span style="color:#d46644">static pods</span></b>

- Only [[7 - Pods ✅|pods]] can be created this way (directly from files in the <i><span style="color:#5c7e3e">manifest</span></i> directory)
	- Since [[5 - Kubelet ✅|kubelet]] works at the [[7 - Pods ✅|pod]] level, it can only understand [[7 - Pods ✅|pods]], which is why it is able to create <b><span style="color:#d46644">static pods</span></b>

### The are two ways to configure the manifest directory

- The <i><span style="color:#5c7e3e">manifest</span></i> directory can be any directory on the host, it just needs to be passed into [[5 - Kubelet ✅|kubelet]] as an option while running the [[10 - Services ✅|service]]
	- This option is called <span style="color:red">--pod-manifest-path</span>

![[static-pods-1.png]]

- Another way to configure the directory is, instead of directly passing the option to the <span style="color:#5c7e3e">kubelet.service</span> file, you could provide the path to another config file using the --config option, and defining the directory path
	- Then in the other file, define the path as the <b><span style="color:#d46644">staticPodPath</span></b> property

![[static-pods-2.png]]

- [[0 - Core Concepts Intro ✅|clusters]] setup by the <b><i><span style="color:#5c7e3e">kubeadm</span></i></b> tool uses this approach.

- To view these files, first <i><span style="color:#5c7e3e">ssh</span></i> into the [[0 - Core Concepts Intro ✅|node]] where the [[7 - Pods ✅|pods]] were deployed

- Then identify the [[5 - Kubelet ✅|kubelet]] config file using the following commands

	Linux:

		ps -aux | grep -i /usr/bin/kubelet

	Mac:

		ps -el | grep -i /usr/bin/kubelet

	- Jot down the <span style="color:red">--config</span> path

	![[static-pods-3.png]]

	- Then lookup the value assigned for <b><span style="color:#d46644">staticPodPath</span></b>:

		`grep -i staticpod KUBELET_CONFIG_FILE_DIR/CONFIG_FILE_NAME`

- Since [[0 - Core Concepts Intro ✅|kubectl]] works by using the [[2 - Kube API server ✅|kube-apiserver]] which is not available for <b><span style="color:#d46644">static pods</span></b>, to view the <span style="color:#5c7e3e">kubelet.service</span> file, you must use the `docker ps` command

![[static-pods-4.png]]

- If there is a [[0 - Core Concepts Intro ✅|cluster]] with an available [[2 - Kube API server ✅|kube-apiserver]], [[5 - Kubelet ✅|kubelet]] can then take in requests for creating [[7 - Pods ✅|pods]] from different inputs
	- The [[7 - Pods ✅|pod]] definition files through the <b><span style="color:#d46644">static pods</span></b> directory
	- An HTTP API endpoint
		- This is how the [[2 - Kube API server ✅|kube-apiserver]] sends input to [[5 - Kubelet ✅|kubelet]]

- [[5 - Kubelet ✅|Kubelet]] can create both kinds of [[7 - Pods ✅|pods]] at the same time

- In the case that [[5 - Kubelet ✅|kubelet]] creates both kinds of [[7 - Pods ✅|pods]], the [[2 - Kube API server ✅|kube-apiserver]] <u><b>IS</b></u> aware of the statically created [[7 - Pods ✅|pods]]

- In this case, if the [[0 - Core Concepts Intro ✅|kubectl]] command is run on the [[0 - Core Concepts Intro ✅|master node]], all [[7 - Pods ✅|pods]] will be listed

- This happens because if [[5 - Kubelet ✅|kubelet]] is part of a [[0 - Core Concepts Intro ✅|cluster]] when creating <b><span style="color:#d46644">static pods</span></b>, it also creates a mirror object in the [[2 - Kube API server ✅|kube-apiserver]]
	- What you see from the [[2 - Kube API server ✅|kube-apiserver]] is a read-only mirror of the [[7 - Pods ✅|pod]]
		- You can view details about the [[7 - Pods ✅|pods]] but you can't edit or delete them like normal [[7 - Pods ✅|pods]]
		- The only way to edit or delete these [[7 - Pods ✅|pods]] is as mentioned above, by modifying or removing them directly from the specified <i><span style="color:#5c7e3e">manifest</span></i> directory

- The name of the <b><span style="color:#d46644">static pod</span></b> is automatically appended with the [[0 - Core Concepts Intro ✅|node]] name

### Why would you use Static pods

- Since <b><span style="color:#d46644">static pods</span></b> are not dependent on the [[0 - Core Concepts Intro ✅|control plane]], you can use <b><span style="color:#d46644">static pods</span></b> to deploy [[0 - Core Concepts Intro ✅|control plane]] components manually as [[7 - Pods ✅|pods]] on the [[0 - Core Concepts Intro ✅|node]]
	- Start by deploying [[5 - Kubelet ✅|kubelet]] on all of the master [[0 - Core Concepts Intro ✅|nodes]] 
	- Then create [[7 - Pods ✅|pod]] definition files that use <i><span style="color:#5c7e3e">docker</span></i> images of the various [[0 - Core Concepts Intro ✅|control plane]] components ([[2 - Kube API server ✅|kube-apiserver]], [[3 - Kube Controller Manager ✅|kube-controller-manager]], [[1 - ETCD ✅|etcd]], [[4 - Kube Scheduler ✅|kube-scheduler]])
	- Place these definition files in the specified <i><span style="color:#5c7e3e">manifest</span></i> directory and [[5 - Kubelet ✅|kubelet]] will take care of the rest
- This method allows you to avoid downloading the binaries and configuring them as [[10 - Services ✅|services]]

- Using this method, if any of the [[7 - Pods ✅|pods]] were to crash, the <b><span style="color:#d46644">static pods</span></b> will automatically be restarted by [[5 - Kubelet ✅|kubelet]]

- This is actually how <b><i><span style="color:#5c7e3e">kubeadm</span></i></b> sets up a <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]]

![[static-pods-5.png]]

#### Practice Problems

- How many <b><span style="color:#d46644">static pods</span></b> exist in this [[0 - Core Concepts Intro ✅|cluster]] in all [[11 - Namespaces ✅|namespaces]]?

		kubectl get pods --all-namespaces

(view the [[7 - Pods ✅|pods]] with a [[0 - Core Concepts Intro ✅|node]] name at the end)

- What is the path of the directory holding the <b><span style="color:#d46644">static pod</span></b> definition files?

		ssh NODE_NAME
		ps -aux | grep -i /usr/bin/kubelet
		(view either the --config or <span style="color:red">--pod-manifest-path</span>)
		grep -i staticpod PATH_NAME

- Create a <b><span style="color:#d46644">static pod</span></b> named "static-busybox" that uses the "busybox" image and the command sleep 1000

		kubectl run static-busybox --restart=Never --image=busybox --dry-run=client -o yaml --command -- sleep 1000 > PATH_NAME/static-busybox-pod.yaml

(--command must be last I think)

- Edit the image on the <b><span style="color:#d46644">static pod</span></b> to use "busybox:1.28.4"

		vi PATH_NAME/static-busybox-pod.yaml
		image: busybox:1.28.4

- Delete the busybox <b><span style="color:#d46644">static pod</span></b>

		rm -f PATH_NAME/static-busybox-pod.yaml