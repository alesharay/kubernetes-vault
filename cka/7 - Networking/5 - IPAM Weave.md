- IP Address Management (IPAM) describes how the virtual bridge networks in the nodes are assigned an IP subnet, how the pods are assigned IP addresses, and who is responsible for ensuring there are no duplicates assigned

- Per CNI, it is the responsibility of the CNI plugin (the network solution provider) to take care of assigning IP address to the containers

- Kubernetes doesn't care how the IP Addresses are managed, it just requires that there are no duplicates and that they are managed properly

- An easy way to manage IP addresses is to store the list of IP addresses in a file and make sure we have the necessary code in the script to manage the file properly

![[ipam-weave-1.png]]

- The IP address file would be placed on each host and manages the IPs of pods on those nodes

![[ipam-weave-2.png]]

- Instead of doing any of the above process manually, the CNI script comes with two built-in plugins to which these tasks can be outsourced (in this case, it is the host_local plugin)
	- It is still our responsibility to invoke that plugin
	- The script can also be dynamic to support different kinds of plugins

- The CNI configuration file has a section called IPAM in which we specify the type of plugin, subnet, and route to be used
	- These details can be read in from the script to invoke the appropriate plugin (instead of hard-coding it)

![[ipam-weave-3.png]]

- Different network solution providers handle CNI configuration in different ways (with the standards being the same)

- Weave, by default, allocates the IP range 10.32.0.0/12 for the entire network

- From the available range, the weave peers will split them equally between each node on the network and pods created on those nodes will have IP addresses in the range specified for their node
	- This is by default but can be changed using additional available options

![[ipam-weave-4.png]]
