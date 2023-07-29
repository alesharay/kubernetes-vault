- As per CNI, container runtimes, in the case Kubernetes, is responsible for creating container network namespaces identifying and attaching those namespaces to the right network by calling the right network plugin

- In order for Kubernetes to use CNI plugins, the CNI plugin must be invoked by the component within Kubernetes that is responsible for creating containers
	- This component must then invoke the appropriate network plugin after the container is created

- The CNI plugin is configured in the kubelet service on each node in the cluster

![[cnik-1.png]]

- You can see the same information on viewing the running kubelet service

![[cnik-2.png]]

- The CNI bin directory has all the supported CNI plugins as executables
	- Such as bridge, dhcp, flannel
		- This directory is /opt/cni/bin by default

![[cnik-3.png]]

- The CNI config directory has a set of configuration files, which is where kubelet looks to find out which plugin needs to be used

![[cnik-4.png]]

   - If there are multiple config files available, choose the one in alphabetical order

- The bridge config file uses a format defined by the CNI standard for a plugin configuration file

![[cnik-5.png]]

- The IPAM type host-local means that the IP addresses are managed locally on this host, unlike a DHCP server maintaining it remotely