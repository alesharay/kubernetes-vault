- The networking at the pod layer is another crucial function of a Kubernetes network

- Kubernetes does not come with a built-in solution for pod networking

- This must be done manually

- Kubernetes has laid out the requirements for pod networking
	- Every pod should have an IP address
	- Every pod should be able to communicate with every other pod in the same node using that same IP address
	- Every pod should be able to communicate with every other pod on other nodes without NAT using that same IP address

- There are many networking solutions out there that can implement these requirements

![[pod-networking-1.png]]

- We create a script for the pod networking and CNI acts as a middleman to run that script automatically

- CNI tells Kubernetes that this is how you should call a script as soon as you create a container, and CNI tells us "this is how your script should look

- Whenever a container is created, the kubelet looks at the CNI configuration passed as a command-line argument when it is run, and identifies the script's name
	- It then looks at the CNI bin directory to find the script
	- It executes the script with the `add` command and the name of the namespace of the container
	- The script takes care of the rest