- Each [[0 - Core Concepts Intro ✅|node]] in a <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]] must have at least 1 [[0.2 - DNS|interface]]
	- Each [[0.2 - DNS|interface]] must have an <span style="color:#5c7e3e">IP address</span> configured
	- The <i><span style="color:#477bbe">host</span></i> must have a unique hostname set and a unique <span style="color:#5c7e3e">MAC address</span>

- There are <span style="color:#5c7e3e">ports</span> on the [[0 - Core Concepts Intro ✅|nodes]] that need to be opened as well, which are used by the various components in the [[0 - Core Concepts Intro ✅|controlplane]]

- The [[0 - Core Concepts Intro ✅|master node]] accepts connections on <span style="color:#5c7e3e">port</span> 6443
	- The [[0 - Core Concepts Intro ✅|worker nodes]], <span style="color:#5c7e3e">kubectl</span> tool, <i><span style="color:#477bbe">external users</span></i>, and all other [[0 - Core Concepts Intro ✅|controlplane]] components access the [[2 - Kube API server ✅|kube-apiserver]] via the 6443 <span style="color:#5c7e3e">port</span>

![[cluster-networking-1.png]]

- [[5 - Kubelet ✅|Kublet]], on the [[0 - Core Concepts Intro ✅|master]] and [[0 - Core Concepts Intro ✅|worker nodes]] listen on <span style="color:#5c7e3e">port</span> 10250

![[cluster-networking-2.png]]

- The [[4 - Kube Scheduler ✅|kube-scheduler]] requires <span style="color:#5c7e3e">port</span> 10259 to be opened

![[cluster-networking-3.png]]

- The [[3 - Kube Controller Manager ✅|kube-controller-manager]] requires <span style="color:#5c7e3e">port</span> 10257 to be opened

![[cluster-networking-4.png]]

- The [[0 - Core Concepts Intro ✅|worker nodes]] expose services on the 30000-32767 <span style="color:#5c7e3e">port range</span>

![[cluster-networking-5.png]]

- The [[1 - ETCD ✅|etcd server]] listens on <span style="color:#5c7e3e">port</span> 2379

![[cluster-networking-6.png]]

- If there are multiple [[0 - Core Concepts Intro ✅|master nodes]], in addition to all of the previously mentioned <span style="color:#5c7e3e">ports</span> being opened, you also need <span style="color:#5c7e3e">port</span> 2380 to be opened so that the [[1 - ETCD ✅|clients]] can communicate with each other

- The list of <span style="color:#5c7e3e">ports</span> required to be opened are listed on the following <span style="color:#5c7e3e">Kubernetes</span> documentation page:
[https://kubernetes.io/docs/reference/networking/ports-and-protocols/](https://kubernetes.io/docs/reference/networking/ports-and-protocols/)

- You can use any of the plugins which are described here:

[https://kubernetes.io/docs/concepts/cluster-administration/addons/](https://kubernetes.io/docs/concepts/cluster-administration/addons/)

[https://kubernetes.io/docs/concepts/cluster-administration/networking/#how-to-implement-the-kubernetes-networking-model](https://kubernetes.io/docs/concepts/cluster-administration/networking/#how-to-implement-the-kubernetes-networking-model)