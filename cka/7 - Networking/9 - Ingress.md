- Ingress helps users access an application using a single externally accessible URL that can be configured to route traffic between different services within the cluster based on the URL path and, at the same time, implement SSL security as well

- Think of Ingress as a layer 7 load balancer that is built into the cluster and can be configured using native Kubernetes primitives (just like any other Kubernetes object)

![[ingress-1.png]]

- Ingress still needs to be exposed in order to be accessible outside of the cluster
	- This means it would need to be published as a NodePort or a cloud native load-balancer (This is just a one-time configuration)

![[ingress-2.png]]

![[ingress-3.png]]

### How would all of this be done manually without Ingress?

- A reverse proxy or load-balancing solution (such as Nginx, HA proxy, traefik, etc..) would be deployed on the Kubernetes cluster

![[ingress-4.png]]

- The chosen solution would then be configured to route traffic through other services
	- This would involve defining URL routes, configuring SSL certificates, etc…

![[ingress-5.png]]

### How is Ingress configured?

- Ingress is implemented in Kubernetes in kind of the same way as manual, as you would first deploy one of the available solutions and then specify a set of rules

- The solution deployed is called an ingress controller

- The rules configured are called ingress resources

![[ingress-6.png]]

- A Kubernetes cluster does not come with an ingress controller by default

### Ingress Controller

- There are many solutions available for ingress
	- Ex. GCE (maintained and supported by the Kubernetes project)
	- Nginx (maintained and supported by the Kubernetes)
	- Contour
	- HA Proxy
	- Traefik
	- Istio
	- Etc…

![[ingress-7.png]]

- An Ingress controller is not just another load-balancer or server
	- The load-balance components are just a part of it but these controllers have additional intelligence built into them to monitor the Kubernetes cluster for new definitions (ingress resources) and configure the server accordingly

		** USING NGINX FOR OUR NOTES **

- An nginx ingress controller is deployed as just another deployment in Kubernetes

![[ingress-8.png]]

- The manifest file type for an ingress controller is Deployment and the image to be used is the one that corresponds with your chosen solution
	- This would be a special build of that service created specifically to be used as an ingress controller in Kubernetes which means it has its own set of requirements

- For nginx, within the image, the nginx program is stored at location /nginx-ingress-controller; therefore that must be passed as the command to start the nginx controller service

![[ingress-9.png]]

- Nginx has a set of configuration options such as the path to store the logs, the keep alive threshold, SSL settings, session timeout, etc…

- In order to decouple the configuration data from the nginx ingress controller image, you must create a configMap object and pass the data into it
	- The configMap object can be blank at this point; we just want to have one created that way it will be easy to modify configuration settings in the future

![[ingress-10.png]]

![[ingress-11.png]]


- Two environment variables that hold the pod name and the namespace that holds the pod is also required
	- The nginx service requires this information to read the configuration data from within the pod

![[ingress-12.png]]

- Lastly, the ports used by the nginx ingress controller must be specified

![[ingress-13.png]]

- After the ingress controller definition file has been created, a service is needed to expose the it to the external world

- A service of type NodePort is created to link the service to the previously created deployment

![[ingress-14.png]]

- In order for the ingress controller to monitor the cluster for ingress resources and configure the underlying server when something is changed, it requires a service account with the correct set of permissions
	- This means we need to create a service account with the correct roles and role bindings

![[ingress-15.png]]

- To summarize, with a deployment of the ingress controller image, a service to expose it, a configMap to feed configuration data, and a service account with the correct permissions to access all of these objects (this includes roles, clusterroles, and rolebindings as needed), an ingress controller should be ready in its simplest form

![[ingress-16.png]]

Ingress Resources

- An ingress resource is a set of rules and configurations applied on the ingress controller
	- E.g. a rule can be configured to say "forward all incoming traffic to a single application" or "route traffic to different applications based on the URL", etc…

![[ingress-17.png]]

- Ingress resources are created using definition files (just like other objects in Kubernetes)

- The apiVersion for the ingress resource definition file is "networking.k8s.io/v1"
	- "extensions/v1beta1" was the previous apiVersion

- The kind for the ingress resource definition file is "Ingress"

- The spec section will be configured differently based on the complexity of the setup

- For a simple, single backend, we have a "defaultBackend" property which Is a dictionary/map that includes the "service" property, under which has the "name" and "port" properties with the "number" property falling under "port"

![[ingress-18.png]]

- The "service" property with the child "name" and "port" properties are used because traffic is routed to the Kubernetes service and not the pods directly
	- If it is a single backend, you don't really have any rules and the "service" property with the child "name" and "port" properties will suffice

- Rules are used when you want to route traffic based on different conditions

![[ingress-19.png]]

- FYI: It is possible to have different domain names that reach a cluster by adding multiple DNS entries all pointing to the same ingress controller

- Within each ingress resource rule, you can handle multiple paths
	- There are rules at the top level for each host or domain name and within each rule, there are different paths to route traffic based on the URL

![[ingress-20.png]]

- When there is more than a single backend, on the "Ingress" definition file under spec, start with a the "rules" property

![[ingress-21.png]]

- The "rules" property is an array, with one of the array items being "http" or "https"

![[ingress-22.png]]

- Under the "http" or "https" property, there is a "paths"  property where you specific a set of "path", "pathType", and "backend" for each available

![[ingress-23.png]]

- The "backend" property (from the previous example) is moved into each item in the "paths" array as a sibling to the "path" and "pathType" properties


![[ingress-24.png]]

- If a user tries to access a URL that does not match any of the available rules, then the user is directed to the service specified as the default backend which means you must remember to deploy a default backend

![[ingress-25.png]]

- Another type of ingress definition configuration is using domain names or hostnames

- To split traffic based on domain name, the "host" property is a sibling of the "http(s)" property

![[ingress-26.png]]

- The "host" property for each rule matches the specified value with the domain name used as the request URL and routes traffic to the appropriate backend

- Notice the difference between splitting by path or endpoint/sub-domain and splitting by host or domain name

![[ingress-27.png]]
