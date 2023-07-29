- Each node in a Kubernetes cluster must have at least 1 interface
	- Each interface must have an IP address configured
	- The host must have a unique hostname set and a unique MAC address

- There are ports on the nodes that need to be opened as well, which are used by the various components in the controlplane

- The master node accepts connections on port 6443
	- The worker nodes, kubectl tool, external users, and all other controlplane components access the kube-apiserver via the 6443 port

![[cluster-networking-1.png]]

- Kubelet, on the master and worker nodes listen on port 10250

![[cluster-networking-2.png]]

- The kube-scheduler requires port 10259 to be opened

![[cluster-networking-3.png]]

- The kube-controller-manager requires port 10257 to be opened

![[cluster-networking-4.png]]

- The worker nodes expose services on the 30000-32767 port range

![[cluster-networking-5.png]]

- The etcd server listens on port 2379

![[cluster-networking-6.png]]

- If there are multiple master nodes, in addition to all of the previously mentioned ports being opened, you also need port 2380 to be opened so that the etcd clients can communicate with each other

- The list of ports required to be opened are listed on the following Kubernetes documentation page:
[https://kubernetes.io/docs/reference/networking/ports-and-protocols/](https://kubernetes.io/docs/reference/networking/ports-and-protocols/)

- You can use any of the plugins which are described here:

[https://kubernetes.io/docs/concepts/cluster-administration/addons/](https://kubernetes.io/docs/concepts/cluster-administration/addons/)

[https://kubernetes.io/docs/concepts/cluster-administration/networking/#how-to-implement-the-kubernetes-networking-model](https://kubernetes.io/docs/concepts/cluster-administration/networking/#how-to-implement-the-kubernetes-networking-model)