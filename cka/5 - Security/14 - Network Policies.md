### Basics of traffic flow and rules

- There are many configurations for an application. Assume you have a web server serving front-end to users and an app server serving back-end API server and database server

![[np-1.png]]

- Consider the following scenario: A user sends a request to the web server , the web server then sends a request to the API server  in the back-end, the API server then fetches data from the database server and then sends the data back to the user

![[np-2.png]]

- There are two types of traffic in the web server - API server - DB server setup: ingress and egress

- For a web server, the traffic coming in from users is an ingress traffic and the outgoing requests from the web server to the API server is an egress traffic

![[np-3.png]]

- When you define ingress and egress, remember you're only looking at the direction in which the traffic originated

- The response back to the user doesn't really matter

- For an API server, it receives ingress traffic from the web server and sends egress traffic to the DB server

- The DB server receives ingress traffic from the API server

![[np-4.png]]

- If required rules to get the web server - API server - DB server setup working were listed, we would have:

- an ingress rule that is required to accept HTTP traffic
- an egress rule to allow traffic from the web server to the API server
- an ingress rule to accept traffic on the API server
- an egress to allow traffic on the DB server
- an ingress rule to accept traffic on the DB server

![[np-5.png]]

### Network security in Kubernetes

- On a cluster with a set of nodes hosting a set of pods and services, each node, pod, and services have their own IP address

![[np-6.png]]

- One of the prerequisites for networking in Kubernetes is that with whatever solution you implement, the pods should be able to communicate with each other without having to configure any additional settings like routes.

- In networking, all pods should, by default, be able to reach each other by their IP addresses, pod names, or services

- *Remember: Kubernetes is configured, by default, with an "All Allow" rule that allows traffic from any pod to any other pod or service

- Using the web server - API server - DB server setup but in Kubernetes, for each component in the application, we deploy a pod: one for the web server, one for the API server, and one for the DB server

- Create services to enable communication between the pods as well as the end users

- If we don't want the front-end web server to be able to communicate with the DB server directly, you would implement a network policy to allow traffic to the DB server only from the API server

- A network policy is another object in the Kubernetes namespace

- You link a network policy to one ore more pods

![[np-7.png]]

- You can define rules within the network policy

- Once the network policy is created, it blocks all other traffic to the pod and only allows traffic that matches the specified rule

- This is only applicable to the pod on which the network policy is applied

- To apply or link a network policy to a pod, use labels and selectors

- To apply a network policy, label the pod and apply the same labels on the <span style="color:red">podSelector</span> field in the network policy

![[np-8.png]]

- After the pod is labeled and the rule is built, under policy types we specify whether the rule is to allow ingress or egress traffic or both

![[np-9.png]]

- Specify the ingress rule that allows traffic from the sending pod to the receiving pod using labels and selectors.

- Then add the port of the sending pod

![[np-10.png]]

- The apiVersion for a network policy object is networking.k8s.io/v1. The kind is NetworkPolicy

- In a network policy object file, under the spec section,  add the podSelectors to apply the policy, then the rule

![[np-11.png]]

- *Remember: Network policies are enforced by the network solution implemented on the Kubernetes cluster

- Not all solutions support network policies; a few of them that are supported are kube-router, Calico, Romana and Weave-net

- Flannel does not

![[np-12.png]]

- Always refer to the network solution's documentation to see support for network policies

- Even in a cluster configured with a solution that does not support network policies, you can still create the policies but they will just not be enforced

- You will not get an error message saying that the network solution does not support network policies