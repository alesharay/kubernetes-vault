- Kubernetes deploys a built-in DNS server by default when a cluster is created
	- If Kubernetes is setup manually, the DNS server is also setup manually

![[dnsk-1.png]]

- As far as DNS is concerned, it is assumed that all pods and services can reach each other using their IP addresses

- Whenever a service is created, the Kubernetes DNS service creates a record for it which maps the name of the service to its IP address

![[dnsk-2.png]]

- Once the service name is mapped to its IP address, any pod in the cluster can use or reach the service by name

![[dnsk-3.png]]

- If the pod and the service are in the same namespace, the pod can access the service by just it's name. But if they are in different namespaces, the pod must use the services full name for access (where the last portion of the name is the namespace)

![[dnsk-4.png]]

- For each namespace, the DNS service creates a subdomain

- All services are group together in another subdomain known as SVC

- To reiterate: all pods and services in the same namespace are grouped together within a subdomain with same name as the namespace and ALL services are also grouped together in another subdomain called svc

![[dnsk-5.png]]

- In addition to the previously mentioned groupings, all services and pods are grouped together into a root domain for the cluster, which is set to cluster.local by default
	- So you can access the service using the URL SERVICE_NAME.NAMESPACE_NAME.svc.cluster.local
	- This is the fully qualified domain name (FQDN) for the service

![[dnsk-6.png]]

- DNS records for pods are not created by default but can be enabled explicitly
	- Once explicitly set, records will be created for pods as well
	- For each pod, Kubernetes generates a name by:
		- Replacing the dots in the pod IP address with dashes
		- The namespace is the same as whichever one the pod is in
		- The type subdomain becomes pod
		- The root domain is always cluster.local

![[dnsk-7.png]]