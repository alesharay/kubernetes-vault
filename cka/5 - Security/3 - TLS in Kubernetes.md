- Root certificates are the CA's public and private keys that are configured on CA servers used to sign server certificates

- Server certificates are public and private key pairs used to secure connectivity

- This is any server that needs to be accessed by a client to perform a task

- Client certificates are configured on client servers and requested by the servers to verify the clients trying to access them

- Use server, root, or client as a naming convention for the certificates for simplicity

![[tlsk-1.png]]

- All communication between nodes in a cluster need to be encrypted

- All interactions between all services and their clients need to be secure

- The two primary requirements for a server to be secure are to have

- All the various services within the cluster to have server certificates
- All the clients to use client certificates to verify they are who they say they are

![[tlsk-2.png]]

Which kubernetes cluster components are servers and which are clients

Servers

- The kube-apiserver is the API server that exposes an HTTPS service that other servers as well as external users use to manage the Kubernetes cluster

- This is a server which requires certificates to secure all communication with its clients
- We name the generated certificate apiserver.crt and private key apiserver.key

![[tlsk-3.png]]

- The names could be different based on who setup the cluster

- The etcd server stores all information about the cluster

- We name the generated certificate etcdserver.crt and private key etcdserver.key

![[tlsk-4.png]]

- The kubelet services on the worker nodes are servers that also expose an HTTPS API endpoint that the kube-apiserver talks to in order to interact with worker nodes

- We name the generated certificate kubelet.crt and private key kubelet.key

![[tlsk-5.png]]

Clients

- The client that access the kube-apiserver is the user's and administrator's kubectl or a REST API

- The admin user requires a certificate to authenticate to the kube-apiserver

- We name the generated certificate admin.crt and private key admin.key

![[tlsk-6.png]]

- The scheduler talks to the kube-apiserver to look for pods that require scheduling and then gets the pods scheduled on the correct worker nodes

- As far as the kube-apiserver is concerned, the scheduler is just another client like the admin user

- The scheduler needs to validate its identity using a client TLS certificate

- It needs its own pair of certificates and keys
- We name the generated certificate scheduler.crt and private key scheduler.key

![[tlsk-7.png]]

- The kube-controller-manager is a client that accesses the kube-apiserver and requires a certificate to authenticate

- We name the generated certificate controller-manager.crt and private key controller-manager.key

![[tlsk-8.png]]

- The kube-proxy is a client that accesses the kube-apiserver and requires a certificate to authenticate

- We name the generated certificate kube-proxy.crt and private key kube-proxy.key

![[tlsk-9.png]]

------------------------------------------------------------------------------

- The servers communicate amongst each other as well

- The kube-apiserver is the only server that talks to the etcd server

- As far as the etcd server is concerned, the kube-apiserver is just another client and thus needs to authenticate the kube-apiserver

- The same keys from serving its own API service can be used (apiservice.crt and apiservice.key) or you can generate new ones specifically for this purpose

- The kube-apiserver talks to the kubelet server on each of the individual nodes

- This is how it monitors the worker nodes
- The same keys from serving its own API service can be used (apiservice.crt and apiservice.key) or you can generate new ones specifically for this purpose

![[tlsk-10.png]]

- To generate certificates for the Kubernetes components, we need a certificate authority (CA) to sign all of these certificates

- Kubernetes requires you to have at least one certificate authority for your cluster

- It is possible to have more than one certificate authority for your cluster

- One for all of the components in the cluster
- Another specifically for etcd

- If etcd has its own CA for the cluster, then the etcd server certificates and the etcd client certificates will be all signed by the etcd server CA

- As we know, the CA has its own certificate and private key

- We name the generated certificate ca.crt and private key ca.key