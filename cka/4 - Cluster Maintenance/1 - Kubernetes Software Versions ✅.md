#flashcards/kubernetes/cka/cluster-maintenance

- We know that when we install the <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]], we install a specific version of <span style="color:#5c7e3e">Kubernetes</span>

- We can see the <span style="color:#5c7e3e">Kubernetes</span> version by running the <span style="color:red">kubectl get nodes</span> command

- The <span style="color:#5c7e3e">Kubernetes</span> release version consist of 3 parts
	1. Major Version
	2. Minor Version
	3. Patch Version

- The <b><span style="color:#d46644">minor versions</span></b> are released every few months with new features and functionalities

- The <b><span style="color:#d46644">patch versions</span></b> are released more often with critical bug fixes

- Similar to most other applications, <span style="color:#5c7e3e">Kubernetes</span> follows a <span style="color:#5c7e3e">standard software release versioning procedure</span>

![[ksoftware-1.png]]

- The first <b><span style="color:#d46644">major version</span></b> of <span style="color:#5c7e3e">Kubernetes</span>, 1.0, was released on July of 2015

- The latest <b><span style="color:#d46644">stable version</span></b> of <span style="color:#5c7e3e">Kubernetes</span> is 1.26.0
	- You will also see alpha and beta releases

- All of the bug fixes and improvements first go into an <b><span style="color:#d46644">alpha</span></b> release (tagged <b><span style="color:#d46644">alpha</span></b> in this release)
	- From there, they make their way to <b><span style="color:#d46644">beta</span></b> release where the code is well tested
	- And finally they make their way to the <b><span style="color:#d46644">main stable</span></b> release

- You can find all of the <span style="color:#5c7e3e">Kubernetes</span> releases in the releases page of the <span style="color:#5c7e3e">Kubernetes</span> <span style="color:#5c7e3e">GitHub</span> repository

	[https://kubernetes.io/releases/](https://kubernetes.io/releases/)

- Download the <span style="color:#5c7e3e">Kubernetes.tar.gz</span> file and extract it to find executables for all the <span style="color:#5c7e3e">Kubernetes</span> components
	- The package has all of the [[0 - Core Concepts Intro ✅|control plane]] components in it

![[ksoftware-2.png]]

- All of the components in the downloaded package have the same version
	- [[2 - Kube API server ✅|kube-apiserver]], [[3 - Kube Controller Manager ✅|controller manager]], [[4 - Kube Scheduler ✅|scheduler]], [[5 - Kubelet ✅|kubelet]], [[6 - Kube Proxy ✅|kube-proxy]]

- There are other components within the control plane that do not have the same version
	- [[0 - Core Concepts Intro ✅|ETCD cluster]], [[8 - CoreDNS in Kubernetes|core DNS server]]