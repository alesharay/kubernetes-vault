- REMEMBER: as the cluster grows, it is not feasible to add every new pod to the /etc/hosts file.
	- The better way to do this (if being done manually) is to create a DNS server, and whenever a new pod is created, a record in the DNS server is added for that pod so that other pods can access the new pod
	- You would also configure the /etc/resolv.conf file in the pod to point to the DNS server so that the new pod can resolve other pods in the cluster

![[coreDNSk-1.png]]

- Kubernetes sort of follows the process of adding pod entries to the DNS server, except the pod names that are mapped to their IP addresses are slightly different

- For pods, Kubernetes forms host names by replacing dots with dashes in the IP address of the pod

![[coreDNSk-2.png]]

- Prior to Kubernetes version 1.12, the DNS server implemented by Kubernetes was known as kube-dns
	- It is useful to know this for when you are working with legacy systems

- With Kubernetes v1.12+, the recommended DNS server is CoreDNS

### How is CoreDNS setup

- The CoreDNS server is deployed as a replicaset within a deployment (for redundancy) in the kube-system namespace in the Kubernetes cluster

![[coreDNSk-3.png]]

- The CoreDNS pods run the coreDNS executable (the same executable ran when deploying CoreDNS manually)

![[coreDNSk-4.png]]

- CoreDNS requires a configuration file and Kubernetes uses a file named Corefile located at /etc/coredns/Corefile

- Within the CoreDNS configuration file (Corefile), there are a number of plugins configured (pictured in orange below) which describe how to handle errors, report health, monitor metrics, cache, etc…

![[coreDNSk-5.png]]

- The plugin that makes CoreDNS work with Kubernetes is the kubernetes plugin
	- This is where the top level domain name for the cluster is configured

![[coreDNSk-6.png]]

- Every record in the coreDNS DNS server will fall under the domain set for the kubernetes plugin in the Corefile

- With the Kubernetes plugin, there are multiple options
	- The pod option is responsible for creating a record for the pods in the cluster (this entry is disabled by default)

- Any record that the DNS server can't solve, it is forwarded to the nameserver specified in the coreDNS pod's /etc/resolv.conf file
	- The /etc/resolv.conf file is set the use the nameserver from the Kubernetes node

- The Corefile is passed into the pod as a configMap
	- This way if you need to modify the configuration, you can edit the configMap

- When the coreDNS pod is up and running and watching the Kubernetes cluster for new pods or services, every time a pod or service is created, it adds a record for it

![[coreDNSk-7.png]]

- When the coreDNS solution is deployed, a service is also created and made available to other components within a cluster

![[coreDNSk-8.png]]

- The coreDNS service is named kube-dns by default and the IP address of the service  is configured as the nameserver on pods

![[coreDNSk-9.png]]

- The DNS configurations on pods are done by kubernetes automatically when pods are created
	- The kubelet is responsible for this

- The config file of the kubelet shows the IP address of the DNS server and domain in it

-  The resolv.conf file also has a 'search' entry that has the following entries so that when you nslookup or search by name, the FQDN will be used automatically
	- NAMESPACE.svc.cluster.local
	- svc.cluster.local
	- cluster.local

- The resolv.conf file only has search entries for services; for pods you must specify the FQDN