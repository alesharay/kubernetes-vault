- <b><i><span style="color:#d46644">Ingress</span></i></b> helps <i><span style="color:#477bbe">users</span></i> access an application using a single externally accessible URL that can be configured to route traffic between different [[10 - Services|services]] within the [[0 - Core Concepts Intro ✅|cluster]] based on the URL path and, at the same time, implement [[3.1 - Certification Creation|SSL]] security as well

- Think of <b><i><span style="color:#d46644">ingress</span></i></b> as a layer 7 [[10 - Services|load balancer]] that is built into the [[0 - Core Concepts Intro ✅|cluster]] and can be configured using native <span style="color:#5c7e3e">Kubernetes</span> primitives (just like any other <span style="color:#5c7e3e">Kubernetes</span> object)

![[ingress-1.png]]

- <b><i><span style="color:#d46644">Ingress</span></i></b> still needs to be exposed in order to be accessible outside of the [[0 - Core Concepts Intro ✅|cluster]]
	- This means it would need to be published as a [[10 - Services|NodePort]] or a cloud native [[10 - Services|load balancer]] (This is just a one-time configuration)

![[ingress-2.png]]

![[ingress-3.png]]

### How would all of this be done manually without Ingress?

- A reverse proxy or [[10 - Services|load balancing]] solution (such as <span style="color:#5c7e3e">NGINX</span>, <span style="color:#5c7e3e">HA Proxy</span>, <span style="color:#5c7e3e">HA</span>, etc..) would be [[9 - Deployments ✅|deployed]] on the <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]]

![[ingress-4.png]]

- The chosen solution would then be configured to route traffic through other [[10 - Services|services]]
	- This would involve defining [[0.1 - Switching, Routing, Gateways CNI in Kubernetes|URL routes]], configuring [[3.1 - Certification Creation|SSL certificates]], etc…

![[ingress-5.png]]

### How is Ingress configured?

- <b><i><span style="color:#d46644">Ingress</span></i></b> is implemented in <span style="color:#5c7e3e">Kubernetes</span> in kind of the same way as manual, as you would first [[9 - Deployments ✅|deploy]] one of the available solutions and then specify a set of rules

- The solution [[9 - Deployments ✅|deployed]] is called an <b><i><span style="color:#d46644">ingress controller</span></i></b>

- The rules configured are called <b><i><span style="color:#d46644">ingress resources</span></i></b>

![[ingress-6.png]]

- A <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]] does not come with an <b><i><span style="color:#d46644">ingress controller</span></i></b> by default

### Ingress Controller

- There are many solutions available for <b><i><span style="color:#d46644">ingress</span></i></b>
	- Ex. <span style="color:#5c7e3e">GCE</span> (maintained and supported by the <span style="color:#5c7e3e">Kubernetes</span> project)
	- <span style="color:#5c7e3e">NGINX</span> (maintained and supported by the <span style="color:#5c7e3e">Kubernetes</span>)
	- <span style="color:#5c7e3e">Contour</span>
	- <span style="color:#5c7e3e">HA Proxy</span>
	- <span style="color:#5c7e3e">HA</span>
	- <span style="color:#5c7e3e">Istio</span>
	- Etc…

![[ingress-7.png]]

- An <b><i><span style="color:#d46644">ingress controller</span></i></b> is not just another [[10 - Services|load balancer]] or <i><span style="color:#477bbe">server</span></i>
	- The [[10 - Services|load balance]] components are just a part of it but these [[3 - Kube Controller Manager ✅|controllers]] have additional intelligence built into them to monitor the <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]] for new definitions (<b><i><span style="color:#d46644">ingress resources</span></i></b>) and configure the <i><span style="color:#477bbe">server</span></i> accordingly

		** USING NGINX FOR OUR NOTES **

- An <span style="color:#5c7e3e">NGINX</span> <b><i><span style="color:#d46644">ingress controller</span></i></b> is [[9 - Deployments ✅|deployed]] as just another [[9 - Deployments ✅|deployment]] in <span style="color:#5c7e3e">Kubernetes</span>

![[ingress-8.png]]

- The manifest file type for an <b><i><span style="color:#d46644">ingress controller</span></i></b> is [[9 - Deployments ✅|deployment]] and the [[11 - Image Security|image]] to be used is the one that corresponds with your chosen solution
	- This would be a special build of that [[10 - Services|service]] created specifically to be used as an <b><i><span style="color:#d46644">ingress controller</span></i></b> in <span style="color:#5c7e3e">Kubernetes</span> which means it has its own set of requirements

- For <span style="color:#5c7e3e">NGINX</span>, within the image, the <span style="color:#5c7e3e">NGINX</span> program is stored at location <span style="color:red">/nginx-ingress-controller</span>; therefore that must be passed as the command to start the <span style="color:#5c7e3e">NGINX controller</span> [[10 - Services|service]]

![[ingress-9.png]]

- <span style="color:#5c7e3e">NGINX</span> has a set of configuration options such as the path to store the [[0 - Managing Application Logs|logs]], the keep alive threshold, [[3.1 - Certification Creation|SSL]] settings, session timeout, etc…

- In order to decouple the configuration data from the <span style="color:#5c7e3e">NGINX</span> <b><i><span style="color:#d46644">ingress controller image</span></i></b>, you must create a [[4 - ConfigMaps|configMap]] object and pass the data into it
	- The [[4 - ConfigMaps|configMap]] object can be blank at this point; we just want to have one created that way it will be easy to modify configuration settings in the future

![[ingress-10.png]]

![[ingress-11.png]]


- Two [[3 - Environment Variables|environment variables]] that hold the [[7 - Pods ✅|pod]] name and the [[11 - Namespaces|namespace]] that holds the [[7 - Pods ✅|pod]] is also required
	- The <span style="color:#5c7e3e">NGINX</span> [[10 - Services|service]] requires this information to read the configuration data from within the [[7 - Pods ✅|pod]]

![[ingress-12.png]]

- Lastly, the <span style="color:#5c7e3e">ports</span> used by the <span style="color:#5c7e3e">NGINX</span> <b><i><span style="color:#d46644">ingress controller</span></i></b> must be specified

![[ingress-13.png]]

- After the <b><i><span style="color:#d46644">ingress controller</span></i></b> definition file has been created, a [[10 - Services|service]] is needed to expose the it to the external world

- A [[10 - Services|service]] of type [[10 - Services|NodePort]] is created to link the [[10 - Services|service]] to the previously created [[9 - Deployments ✅|deployment]]

![[ingress-14.png]]

- In order for the <b><i><span style="color:#d46644">ingress controller</span></i></b> to monitor the [[0 - Core Concepts Intro ✅|cluster]] for <b><i><span style="color:#d46644">ingress resources</span></i></b> and configure the underlying <i><span style="color:#477bbe">server</span></i> when something is changed, it requires a [[10 - Services|service]] account with the correct set of permissions
	- This means we need to create a [[10 - Services|service]] account with the correct [[8 - Role Based Access Controls|roles]] and [[8 - Role Based Access Controls|bindings]]

![[ingress-15.png]]

- To summarize, with a [[9 - Deployments ✅|deployment]] of the <b><i><span style="color:#d46644">ingress controller</span></i></b> image, a [[10 - Services|service]] to expose it, a [[4 - ConfigMaps|configMap]] to feed configuration data, and a [[10 - Service Accounts|service account]] with the correct permissions to access all of these objects (this includes [[8 - Role Based Access Controls|roles]], [[8 - Role Based Access Controls|clusterroles]], and [[8 - Role Based Access Controls|clusterrolebindings]] as needed), an <b><i><span style="color:#d46644">ingress controller</span></i></b> should be ready in its simplest form

![[ingress-16.png]]

Ingress Resources

- An <b><i><span style="color:#d46644">ingress resource</span></i></b> is a set of rules and configurations applied on the <b><i><span style="color:#d46644">ingress controller</span></i></b>
	- E.g. a rule can be configured to say "forward all incoming traffic to a single application" or "[[0.1 - Switching, Routing, Gateways CNI in Kubernetes|route]] traffic to different applications based on the URL", etc…

![[ingress-17.png]]

- <b><i><span style="color:#d46644">Ingress resources</span></i></b> are created using definition files (just like other objects in <span style="color:#5c7e3e">Kubernetes</span>)

- The <span style="color:#5c7e3e">apiVersion</span> for the <b><i><span style="color:#d46644">ingress resource</span></i></b> definition file is "<span style="color:red">networking.k8s.io/v1</span>"
	- "extensions/v1beta1" was the previous <span style="color:#5c7e3e">apiVersion</span>

- The <span style="color:#5c7e3e">kind</span> for the <b><i><span style="color:#d46644">ingress resource</span></i></b> definition file is "<span style="color:red">ingress</span>"

- The <span style="color:#5c7e3e">spec</span> section will be configured differently based on the complexity of the setup

- For a simple, single backend, we have a "<span style="color:red">defaultBackend</span>" property which Is a <span style="color:#5c7e3e">dictionary</span>/<span style="color:#5c7e3e">map</span> that includes the "<span style="color:red">service</span>" property, under which has the "<span style="color:red">name</span>" and "<span style="color:red">port</span>" properties with the "<span style="color:red">number</span>" property falling under "<span style="color:red">port</span>"

![[ingress-18.png]]

- The "<span style="color:red">service</span>" property with the child "<span style="color:red">name</span>" and "<span style="color:red">port</span>" properties are used because traffic is routed to the <span style="color:#5c7e3e">Kubernetes</span> service and not the [[7 - Pods ✅|pods]] directly
	- If it is a single backend, you don't really have any rules and the "<span style="color:red">service</span>" property with the child "<span style="color:red">name</span>" and "<span style="color:red">port</span>" properties will suffice

- Rules are used when you want to [[0.1 - Switching, Routing, Gateways CNI in Kubernetes|route]] traffic based on different conditions

![[ingress-19.png]]

- FYI: It is possible to have different [[0.2 - DNS|domain names]] that reach a [[0 - Core Concepts Intro ✅|cluster]] by adding multiple [[0.2 - DNS|DNS]] entries all pointing to the same <b><i><span style="color:#d46644">ingress controller</span></i></b>

- Within each <b><i><span style="color:#d46644">ingress resource</span></i></b> rule, you can handle multiple paths
	- There are rules at the top level for each <i><span style="color:#477bbe">host</span></i> or [[0.2 - DNS|domain name]] and within each rule, there are different paths to [[0.1 - Switching, Routing, Gateways CNI in Kubernetes|route]] traffic based on the URL

![[ingress-20.png]]

- When there is more than a single backend, on the "<span style="color:red">ingress</span>" definition file under <span style="color:#5c7e3e">spec</span>, start with a the "<span style="color:red">rules</span>" property

![[ingress-21.png]]

- The "<span style="color:red">rules</span>" property is an <span style="color:#5c7e3e">array</span>, with one of the <span style="color:#5c7e3e">array</span> items being "<span style="color:red">http</span>" or "<span style="color:red">https</span>"

![[ingress-22.png]]

- Under the "<span style="color:red">http</span>" or "<span style="color:red">https</span>" property, there is a "<span style="color:red">paths</span>"  property where you specific a set of "<span style="color:red">path</span>", "<span style="color:red">pathType</span>", and "<span style="color:red">backend</span>" for each available

![[ingress-23.png]]

- The "<span style="color:red">backend</span>" property (from the previous example) is moved into each item in the "<span style="color:red">paths</span>" <span style="color:#5c7e3e">array</span> as a sibling to the "<span style="color:red">path</span>" and "<span style="color:red">pathType</span>" properties


![[ingress-24.png]]

- If a <i><span style="color:#477bbe">user</span></i> tries to access a URL that does not match any of the available rules, then the <i><span style="color:#477bbe">user</span></i> is directed to the service specified as the default backend which means you must remember to deploy a default backend

![[ingress-25.png]]

- Another type of <b><i><span style="color:#d46644">ingress</span></i></b> definition configuration is using [[0.2 - DNS|domain names]] or hostnames

- To split traffic based on [[0.2 - DNS|domain name]], the "<span style="color:red">host</span>" property is a sibling of the "<span style="color:red">http(s)</span>" property

![[ingress-26.png]]

- The "<span style="color:red">host</span>" property for each rule matches the specified value with the [[0.2 - DNS|domain name]] used as the request URL and routes traffic to the appropriate backend

- Notice the difference between splitting by path or <span style="color:red">endpoint/subdomain</span> and splitting by <i><span style="color:#477bbe">host</span></i> or [[0.2 - DNS|domain name]]

![[ingress-27.png]]
