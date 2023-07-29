- In the past, Kubernetes used Docker alone as the container runtime engine

- This means that all of the code to work with Docker was embedded into the Kubernetes source code

- Container Runtime Interface (CRI) came to be because with other container runtime engines being introduced (such as rkt and cri-o), it was important to open up and extend support to work with different container runtimes and not be dependent on Kubernetes source code

- Container Runtime Interface (CRI) is a standard that defines how an orchestration solution (like Kubernetes) would communicate with container runtimes (like Docker)

![[container-storage-1.png]]

- In the future, if any new container runtime is developed, they can simply follow the CRI standards

- To extend support for different networking solutions, the Container Networking Interface (CNI) was introduced

- The CNI allows any new networking vendors to simply develop plugin based on the CNI standards and make their solution work with Kubernetes

![[container-storage-2.png]]

- The Container Storage Interface supports multiple storage solutions

![[container-storage-3.png]]

- With CSI, you can now write your own drivers for your own storage to work with Kubernetes

- CSI is not a Kubernetes specific standard as it is meant to be a universal standard that if implemented, allows any container orchestration tool to work with any storage vendor with a supported plugin

- Currently, Kubernetes, CloudFoundry, and Mesos are on board with CSI

- The CSI defines a set of remote procedure calls (RPCs) that will be called by the container orchestrator and must be implemented by storage drivers

- Ex. CSI says that when a pod is created and requires a volume, the container orchestrator should call the <span style="color:red">create volume</span> RPC and pass a set of details such as the volume name

- After calling the <span style="color:red">create volume</span> RPC, the storage driver should implement this RPC, handle the request, provision a new volume on the storage array and return the results of the operation

- Similar to the <span style="color:red">create volume</span> RPC, the container orchestrator should call the <span style="color:red">delete volume</span> RPC when a volume is to be deleted and the storage driver should implement the code to decommission the volume from the array when that call is made

- The specification details exactly what parameters should be passed by the caller, what should be received by the solution and what error codes should be exchanged