- Most upgrade information can be found on the following page

[https://v1-20.docs.kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/](https://v1-20.docs.kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/)

- It is not mandatory for all of the core controlplane components to have the same version

![[clusteru-1.png]]

- Since the kube-apiserver is the most important component in the controlplane and is the component that all other components must talk to, none of the components can have a higher version than the kube-apiserver

- The controller-manager and scheduler can be one version lower than the kube-apiserver

- The kubelet and kube-proxy can be two versions lower than the kube-apiserver

![[clusteru-2.png]]

- The versioning rules that apply to the control plane components do not apply to kubectl

- The kubectl utility can be one version higher, the same, or one version lower than the kube-apiserver

![[clusteru-3.png]]

- This permissible skew in versions allow live upgrades to be carried out

### When should you upgrade

- At any time, Kubernetes only supports up to three minor versions

- Be sure to upgrade your current version before the next version (which will kick your current version to unsupported status) is released

![[clusteru-4.png]]

- The recommended method of upgrading is to upgrade one minor version at a time

- As opposed to just jumping to the latest version

- The upgrade process depends on how your cluster is setup

- If the cluster is managed on a cloud platform, there are likely tools that make upgrading easy
- If the cluster is managed with tools such as kubeadm, the tool will help you plan and upgrade the cluster

![[clusteru-5.png]]

- If the cluster was deployed from scratch, you must manually upgrade each component in the cluster

### Upgrading a Kubeadm Cluster

- Upgrading a cluster involves two major steps:

- Upgrade the master nodes
- Upgrade the worker nodes

- While the master node is being upgraded, the control plane components such as the kube-apiserver, scheduler, and controller-managers go down briefly

- The master node going down doesn't mean your worker nodes and applications on the cluster are impacted

- Since the master node is down, all management functions are down

- In this case, you cannot access the cluster using kubectl or the Kubernetes API
- You cannot deploy new applications or modify existing ones
- The controller-managers won't function either

- If a pod was to fail, a new pod will not be automatically brought back up

- Once the upgrade is complete and the cluster is back up, it should function as normal

- There are different strategies available to upgrade the worker nodes

- Strategy 1 to upgrade a worker node is to upgrade all of them at once

- With this process, all pods are down and the application is unavailable to your users
- Once the upgrade is complete, the nodes are back up, the new pods are scheduled, and users are able to access your application

- Strategy 2 to upgrade a worker node is to upgrade them one at a time

- While upgrading one node, the pods will be placed on other nodes while the current one is being upgraded
- Once the first node is back up, the second node goes down and those pods are then moved to the other nodes while that one is being upgraded
- This process is repeated until all worker nodes are upgraded

- Strategy 3 to upgrade a worker node is to add new nodes (with newer versions) to the cluster

- This method is convenient if you're on a cloud environment
- Move the workload from the first node to the new node and terminate the original node
- Continue this until you have all new upgraded nodes with the pods moved to them

- Kubeadm has an upgrade command that helps in upgrading clusters

- First run the <span style="color:red">kubeadm upgrade plan</span> command

![[clusteru-6.png]]

- This gives a bunch of information like the cluster version and kubeadm tool version
- This also lists all of the control plane components, their current versions, and what versions they can be upgraded to
- It also tells you what components need to be upgraded manually
- Keep in mind that kubeadm does not install or upgrade kubelet
- Finally, the command gives you the command to upgrade the cluster

- You must upgrade the kubeadm tool first before you can upgrade the cluster

- The kubeadm tool follows the same software version as Kubernetes

- First upgrade the kubeadm tool with the OS's upgrade tool (ie. apt/yum upgrade)

- Then after upgrading the cluster, upgrade kubelet with the OS's upgrade tool

- Run the <span style="color:red">kubeadm upgrade apply</span>command with the version

- Be sure to upgrade one version at a time

![[clusteru-7.png]]

- The version seen when running the <span style="color:red">kubectl get nodes</span> is showing the version of kubelet on each of the nodes registered with the kube-apiserver and not the version of the kube-apiserver itself

- So when you first upgrade and then run the <span style="color:red">kubectl get nodes</span> command, you will still see an older kubelet version

![[clusteru-8.png]]

- Then upgrade the kubelet version

![[clusteru-9.png]]

- Keep in mind that the drain, cordon, and uncordon commands are only to be run in the control plane node (passing in any node name - master) since it is a kubectl command

- If it is run on a worker node, you will receive an error

### Practice Problems

- What is the current version of the cluster

		kubectl get nodes

- You are tasked to upgrade the cluster. User's accessing the applications must not be impacted. And you cannot provision new VMs.

What strategy would you use to upgrade the cluster

You drain one node, allow the pods to go to the other, and upgrade the current node

- What is the latest stable version available for upgrade?

		kubeadm upgrade plan

- Drain the master node and mark it as unschedulable

		kubectl drain controlplane --ignore-daemonsets

- Upgrade the controlplane components to exact version v1.20.0

	1. `apt update && \`
	    `apt install -y --allow-change-held-packages kubeadm=1.20.0`

	1. `kubeadm upgrade plan`

	1. `kubeadm upgrade apply v1.20.0`

	1. `apt update && \``

apt install -y --allow-change-held-packages kubelet=1.20.0 kubectl=1.20.0

	1. `systemctl daemon-reload`
	    `systemctl restart kubelet`

	1. `kubectl get nodes`     # checking status

- Mark the controlplane node as schedulable again

		kubectl uncordon controlplane

- Now do the same and upgrade the worker node "node01" to version 1.20.0

(from control plane)

		kubectl drain node01 --ignore-daemonsets

ssh node01

	1. apt update && \
		  apt install -y --allow-change-held-packages kubeadm=1.20.0

	1. kubeadm upgrade plan

	1. kubeadm upgrade node

	1. apt update && \

apt install -y --allow-change-held-packages kubelet=1.20.0 kubectl=1.20.0

	1. systemctl daemon-reload && \  
    systemctl restart kubelet

	1. kubectl get nodes     # checking status

- Remove the restriction and mark node01 as schedulable again

		kubectl uncordon node01