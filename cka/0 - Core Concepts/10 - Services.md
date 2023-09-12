- <b><span style="color:#d46644">Services</span></b> enable communication between various components within and outside of the application
	- This helps us connect applications together with other applications or users

- <b><span style="color:#d46644">Services</span></b> enable <span style="color:#5c7e3e">loose coupling</span> between <b><span style="color:#d46644">microservices</span></b> in our application

- <b><span style="color:#d46644">Services</span></b> are the middlemen that help us map requests from our local machines, through the [[0 - Core Concepts Intro ✅|nodes]], and to the [[7 - Pods|pods]]

- <b><span style="color:#d46644">Services</span></b> are k8s objects (like [[7 - Pods|pods]], [[9 - Deployments|deployments]], [[8 - ReplicaSets|replicaSets]], etc…)

- The <b><i><span style="color:#d46644">NodePort Service</span></i></b> listens to a <span style="color:#5c7e3e">port</span> on the [[0 - Core Concepts Intro ✅|node]] and forwards requests on that <span style="color:#5c7e3e">port</span> to a <span style="color:#5c7e3e">port</span> on the [[7 - Pods|pod]] (which runs the given application)

- The <b><i><span style="color:#d46644">ClusterIP service</span></i></b> creates a virtual IP inside the [[0 - Core Concepts Intro ✅|cluster]] to enable communication between the different <b><span style="color:#d46644">services</span></b> on the [[0 - Core Concepts Intro ✅|cluster]] (ie. Set of front-end servers to a set of back-end servers)

- The <b><i><span style="color:#d46644">LoadBalancer Service</span></i></b> provisions a <b><span style="color:#d46644">load balancer</span></b> for the application in supported cloud providers (ie. Distributing loads across different web servers in your front-end tier)

![[services-1.png]]

- To create a <b><span style="color:#d46644">service</span></b>, we use a definition file similar to any other <span style="color:#5c7e3e">Kubernetes</span> object, with the same high-level requirements (<i><span style="color:#477bbe">apiVersion, kind, metadata, spec</span></i>)

- The <i><span style="color:#477bbe">spec</span></i> requirements for a <b><span style="color:#d46644">service</span></b> are:
	- <b><span style="color:#5c7e3e">type</span></b>
		- This is the type of <b><span style="color:#d46644">service</span></b> we are creating (<b><span style="color:#d46644">NodePort, ClusterIP, LoadBalancer</span></b>)
	- <b><span style="color:#5c7e3e">ports</span></b>
		- The <span style="color:#5c7e3e">ports</span> that will need to be open to be communicated with (using above example)
			- <b><span style="color:#5c7e3e">targetPort</span></b>: 80
				- Optional: If this is not provided, it is assumed to be the same as port
			- <b><span style="color:#5c7e3e">port</span></b> (service port): 80
				- required
			- <b><span style="color:#5c7e3e">nodePort</span></b>: 30008
				- Optional: if this is not provided, an available <span style="color:#5c7e3e">port</span> within the valid range will be automatically assigned
	- <span style="color:#5c7e3e">selector</span>
				- This connects the <b><span style="color:#d46644">service</span></b> to the [[7 - Pods|pod]]
		- Under the [[1 - Labels & Selectors|selector]], a list of [[1 - Labels & Selectors|labels]] are provided that will identify the [[7 - Pods|pods]]

- Note that <span style="color:#5c7e3e">port</span> is an <b><i><span style="color:#5c7e3e">array/list</span></i></b>, as you can have multiple <span style="color:#5c7e3e">port</span> mappings in each <b><span style="color:#d46644">service</span></b>

- To create a <b><span style="color:#d46644">service</span></b>, configure a <b><span style="color:#d46644">Service</span></b> definition file
	- apiVersion: v1
	- kind: Service
	- metadata:
		- Name
		- Labels (optional)
	- spec:
		- type:
		- ports:
		- Selector:
			- Use [[1 - Labels & Selectors|pod labels]] here to link the <b><span style="color:#d46644">service</span></b> to the [[7 - Pods|pod]]

![[services-2.png]]

- Use the <span style="color:red">kubectl create -f</span>  command to create the <b><span style="color:#d46644">service</span></b>

		kubectl create -f <service_definition_file_name>

- Use the <span style="color:red">kubectl get services</span> command to see a list of the <b><span style="color:#d46644">service</span></b>

		kubectl get services

- NOTE: a <b><span style="color:#d46644">service</span></b> is only necessary for an application if it needs to be accessed by some other <b><span style="color:#d46644">service</span></b>/<i><span style="color:#477bbe">object</span></i>.
	- If it only needs to access other <i><span style="color:#477bbe">objects</span></i> but is not being accessed, no <b><span style="color:#d46644">service</span></b> is necessary

### Nodeport

- This is the most primitive way to expose a <b><span style="color:#d46644">service</span></b> to the internet

- The <b><span style="color:#d46644">service</span></b> is like a virtual server on the [[0 - Core Concepts Intro ✅|cluster]] and has it's own <span style="color:#5c7e3e">IP</span> address (<b><span style="color:#d46644">ClusterIP</span></b>)

- There are three <span style="color:#5c7e3e">ports</span> open
	- <span style="color:#5c7e3e">Port</span> on the [[0 - Core Concepts Intro ✅|node]]
		- This is referred to as the <b><span style="color:#d46644">NodePort</span></b> as this <span style="color:#5c7e3e">port</span> is open on the [[0 - Core Concepts Intro ✅|node]] itself and is used to access the application server
		- These can only be in a specific range (30000-32767)
	- <span style="color:#5c7e3e">Port</span> on the <b><span style="color:#d46644">Service</span></b>
		- This is simply referred to as the <span style="color:#5c7e3e">Port</span>
	- <span style="color:#5c7e3e">Port</span> on the [[7 - Pods|Pod]]
		- This is referred to as the <b><span style="color:#d46644">TargetPort</span></b> as that is where the <b><span style="color:#d46644">service</span></b> forwards the requests to

![[services-3.png]]

### Multiple Pods and/or Multiple Nodes

- When the <b><span style="color:#d46644">service</span></b> is created, it looks for a matching [[7 - Pods|pod]] with the same [[1 - Labels & Selectors|label]] as the [[1 - Labels & Selectors|label selector]]
	- For all of the matching [[7 - Pods|pods]] that it finds, the <b><span style="color:#d46644">service</span></b> will automatically select them  as endpoints for forwarding requests coming from the <i><span style="color:#477bbe">user</span></i>
	- No additional configuration is necessary making this happen

- When [[7 - Pods|pods]] are distributed across multiple [[0 - Core Concepts Intro ✅|nodes]] and a <b><span style="color:#d46644">service</span></b> is created, k8s automatically creates a <b><span style="color:#d46644">service</span></b> that spans all of the [[0 - Core Concepts Intro ✅|nodes]] in the [[0 - Core Concepts Intro ✅|cluster]] and maps the <span style="color:#5c7e3e">TargetPort</span> to the same <span style="color:#5c7e3e">port</span> on all of the [[0 - Core Concepts Intro ✅|nodes]] in the [[0 - Core Concepts Intro ✅|cluster]] 
	- This allows you to access your application using the <span style="color:#5c7e3e">IP</span> of any [[0 - Core Concepts Intro ✅|node]] in the [[0 - Core Concepts Intro ✅|cluster]] and using the same <b><span style="color:#d46644">NodePort</span></b> number

- This means that regardless of your setup being a single [[7 - Pods|pod]] on a single [[0 - Core Concepts Intro ✅|node]], multiple [[7 - Pods|pods]] on a single [[0 - Core Concepts Intro ✅|node]], or multiple [[7 - Pods|pods]] on multiple [[0 - Core Concepts Intro ✅|nodes]], the <b><span style="color:#d46644">service</span></b> is created exactly the same without you having to do anything
	- When [[7 - Pods|pods]] are removed or added, the <b><span style="color:#d46644">service</span></b> is automatically updated

### ClusterIP

- The <b><span style="color:#d46644">ClusterIP service</span></b> creates a virtual <b><u><span style="color:#5c7e3e">IP</span></u></b> address inside of the [[0 - Core Concepts Intro ✅|cluster]] to enable communication between the different <b><span style="color:#d46644">services</span></b> in the [[0 - Core Concepts Intro ✅|cluster]] (ie. Set of front-end <b><span style="color:#d46644">service</span></b> to a set of back-end servers)
	- There is <b><u><span style="color:#5c7e3e">no</span></u></b> external access

- As [[7 - Pods|pods]] are created and terminated, the <span style="color:#5c7e3e">IP</span> addresses are subject to change and as such, cannot be relied on as a valid way to establish communication between them

- The <b><span style="color:#d46644">ClusterIP service</span></b> can help group [[7 - Pods|pods]] together and provide a single interface to access the [[7 - Pods|pods]] in a group

- For the grouped [[7 - Pods|pods]], a single interface will be provided for other [[7 - Pods|pods]] to access this <b><span style="color:#d46644">service</span></b>
	- The requests that come in are then forwarded to a random [[7 - Pods|pod]] within the group

- Each <b><span style="color:#d46644">service</span></b> gets an <span style="color:#5c7e3e">IP</span> and name inside of the [[0 - Core Concepts Intro ✅|cluster]] and that is the name used to communicate with from other [[7 - Pods|pods]]

- The <b><span style="color:#d46644">ClusterIP service</span></b> is the default <b><span style="color:#d46644">service</span></b>.
	- Thus, if no type is given in the <b><span style="color:#d46644">service</span></b> definition file, then it will automatically be given a type of <b><span style="color:#d46644">ClusterIP</span></b>

- The <b><span style="color:#d46644">service</span></b> can be accessed using the <b><span style="color:#d46644">ClusterIP</span></b> address or the <b><span style="color:#d46644">service</span></b> name

### LoadBalancer

- The <b><span style="color:#d46644">LoadBalancer service</span></b> provisions a <b><span style="color:#d46644">load balancer</span></b> for the application using supported cloud providers (ie. AWS, GCP, Azure), distributing loads across different web servers in your front-end tier
	- This is the standard way to expose a <b><span style="color:#d46644">service</span></b> to the internet

- Even if one <b><span style="color:#d46644">microservice</span></b> is only on some of the [[0 - Core Concepts Intro ✅|nodes]] in the [[0 - Core Concepts Intro ✅|cluster]] (ie. Voting app on two [[0 - Core Concepts Intro ✅|nodes]] and registration app on two [[0 - Core Concepts Intro ✅|nodes]]), all <b><span style="color:#d46644">microservices</span></b> will be accessible on all [[0 - Core Concepts Intro ✅|nodes]] in the [[0 - Core Concepts Intro ✅|cluster]] because that's how <b><span style="color:#d46644">services</span></b> work

- However, we don't want to give the end-users every <span style="color:#5c7e3e">IP</span> address and <span style="color:#5c7e3e">port</span> for all available [[0 - Core Concepts Intro ✅|nodes]]. We want to give them one <span style="color:#5c7e3e">IP</span> address that will always be able to access the entire application

- In this case, we would want to setup a <b><span style="color:#d46644">load balancer</span></b> for all available [[7 - Pods|pods]] / [[0 - Core Concepts Intro ✅|nodes]] and give end-users the address of the <b><span style="color:#d46644">load balancer</span></b>

- When the <b><span style="color:#d46644">load balancer</span></b> <span style="color:#5c7e3e">IP</span> address is reached, it will automatically send the request to the <b><span style="color:#d46644">nodePort service</span></b> which will then send the request to all [[7 - Pods|pods]] matching the given [[1 - Labels & Selectors|selector]]

- This will only work in supported cloud platforms.
	- If set in a non-supported platform, it will be treated like a <b><span style="color:#d46644">nodePort service</span></b>

[From Kubernetes for Beginners - Services](onenote:Kubernetes%20for%20Beginners.one#Services&section-id={5D5A45D8-45DB-1442-8EBF-F2131933F0D4}&page-id={7A83C485-745B-E44F-821C-06F831D6AA07}&end&base-path=https://d.docs.live.net/a8ff567768035d78/Documents/Kubernetes)