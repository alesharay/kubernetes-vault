Steps for 3 Node (one master, two workers) Setup

✅ ❌

1. We need to have multiple servers or VMs provisioned ✅
	1. These can be virtual or physical

2. Once all nodes are provisioned, designate one as the master and the remaining as the workers ✅

3. Install a container runtime on all nodes (master and worker)  ✅

4. Install Kubeadm on all 3 nodes  ✅
	1. This will help us bootstrap the Kubernetes solution by installing all necessary components on the right nodes in the right order

5. Initialize the master server  ✅

6. Once the master server has been initialized but prior to joining the worker nodes to cluster, we must ensure that the networking prerequisites are met  ✅
	1. As we know, Kubernetes requires a special networking solution between the master and worker nodes called the pod network

7. Once the pod network is setup, we can join the worker nodes to the cluster  ✅

8. Once all of this is done, the cluster is ready for us to deploy our application  ✅