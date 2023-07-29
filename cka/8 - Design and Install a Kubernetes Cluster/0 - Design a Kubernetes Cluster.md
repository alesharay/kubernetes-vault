- When designing a Kubernetes cluster, ask yourself these questions:
	- Purpose
		- Education
		- Development & Testing
		- Hosting Production Applications
	- Cloud or OnPrem?
	- Workloads
		- How many?
		- What kind?	
			- Web
			- Big Data/Analytics
		- Application Resource Requirements
			- CPU Intensive
			- Memory Intensive
		- Traffic
			- Heavy traffic
			- Burst traffic

### Purpose

- If you want to deploy a cluster for learning purposes, then a single node solution such as Minikube, kind,  or a small Kubeadm/GCP/AWS/Azure instance should do

- If you want to deploy a cluster for development of testing purposes, a multi-node cluster with a single master and multiple workers would help
	- In this case, Kubeadm or quick provisioning on a cloud platform would work

- For hosting a production grade application, a High Availability, multi-node cluster with multiple master nodes would be appropriate

- Production grade applications can also be setup with Kubeadm or a cloud platform
	- E.g. up to 5000 nodes
	- Up to 150,000 pods in the cluster
	- Up to 300,000 total containers
	- Up to 100 pods per node

- Depending on the size of your cluster, the resource requirements will vary

![[design-1.png]]

### Cloud or OnPrem?

- For OnPrem, Kubeadm is a very useful tool

- Depending on the workload, your node and disk configurations will differ

![[design-2.png]]

![[design-3.png]]

- Typically, you have all of the controlplane components on the master nodes, however, in large cluster configurations, you may want to separate your etcd clusters into their own cluster nodes

![[design-4.png]]
