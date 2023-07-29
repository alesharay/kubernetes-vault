- The WeaveWorks weave CNI plugin is one of many solutions based on CNI

- With a network solution that is manually configured with a routing table which maps what networks are on what hosts, when a packet is sent from one pod to another, it goes out to the network, to the router and finds it's way to the node that hosts that pod
	- This only works for smaller environments

- When the weave CNI plugin is deployed on a cluster, it deploys a service on each node.
	- These services communicate with each other to exchange information regarding the nodes, networks and pods within them

![[cni-weave-1.png]]

- Each weave plugin service (or peer) stores a topology of the entire setup, this way they know what pods and their IP addresses on all other nodes

- Weave creates it's own bridge on the nodes and names it weave

- After the weave plugin creates the weave bridge on the node, it assigns IP addresses to each node in the  network

![[cni-weave-2.png]]

- REMEMBER: a single pod may be attached to multiple bridge networks

- The path a packet takes to reach it's destination depends on the route configured on the container

- Weave makes sure the pods get the correct route configured to reach the service and the service takes care of the rest

- When a packet is sent to a pod on another node, weave intercepts the packet and identifies that it is on a separate network, encapsulates the packet into a new one with new source and destination and sends it across the network

- Once the packet is on the other network, the other weave service retrieves the packet, decapsulates and routes the packet to the correct pod

- To deploy weave on a Kubernetes cluster, it can be deployed as services or daemons on each node in the cluster manually
	- If a cluster already exists, the easier way is to deploy as pods in the cluster

- Once the base Kubernetes system is ready with nodes, networking is configured correctly between the nodes, and the basic controlplane components are deployed, weave can be deployed in the cluster with a simple command:

kubectl apply -f "[https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')](https://cloud.weave.works/k8s/net?k8s-version=$(kubectl%20version%20|%20base64%20|%20tr%20-d%20'\n'))"

- When weave is applied, it deploys all the necessary components required for weave in the cluster

- The weave peers are deployed as a daemonset which ensures that one weave pod  is deployed on each node in the cluster