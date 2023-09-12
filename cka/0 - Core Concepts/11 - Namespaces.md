- The <i><span style="color:#477bbe">default</span></i> <b><span style="color:#d46644">namespaces</span></b> is created automatically with <span style="color:#5c7e3e">Kubernetes</span>

- When a [[0 - Core Concepts Intro ✅|cluster]] is first setup, <span style="color:#5c7e3e">Kubernetes</span> creates [[0 - Core Concepts Intro ✅|cluster]] and [[10 - Services|services]] for its internal purpose such as those required by the networking solution
	- To isolate these from the user, or preventing them from modifying these [[10 - Services|services]], <span style="color:#5c7e3e">Kubernetes</span> creates them in another <b><span style="color:#d46644">namespace</span></b> at startup name  <b><span style="color:#d46644">kube-system</span></b>

- A third <b><span style="color:#d46644">namespace</span></b> created by <span style="color:#5c7e3e">Kubernetes</span> automatically is the <b><span style="color:#d46644">kube-public namespace</span></b>
	- This is where resources that should be made available to all users are created

- The resources within a <b><span style="color:#d46644">namespace</span></b> can refer to each other simply by their names

- To refer to a resource in another <b><span style="color:#d46644">namespace</span></b>, you must append the name of the <b><span style="color:#d46644">namespace</span></b> to the request
	- Do this by using the <i><span style="color:#477bbe">SERVICE_NAME.NAMESPACE.svc.cluster.local</span></i> format
	- This is possible because when the [[10 - Services|service]] is created, a <b><span style="color:#5c7e3e">DNS</span></b> entry is added automatically in this format
	- <i><span style="color:#477bbe">cluster.local</span></i> is the default domain name of the <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]]
		- <span style="color:#5c7e3e">svc</span> is the subdomain name

		`RESOURCE_NAME.NAMESPACE.SUBDOMAIN.DOMAIN`

- To list [[7 - Pods|pods]] in another <b><span style="color:#d46644">namespace</span></b>, use the <span style="color:red">--namespace NAMESPACE</span> OR <span style="color:red">-n NAMESPACE</span> option

		kubectl get pods --namespace dev

	- Same with any other common command

- To list the <i><span style="color:#477bbe">DNS</span></i> ([[10 - Services|service]]) on a <b><span style="color:#d46644">namespace</span></b>, use the <span style="color:red">kubectl get service</span> command

- You can also specify the <b><span style="color:#d46644">namespace</span></b> as a <i><span style="color:#477bbe">metadata</span></i> property in a <b><i><span style="color:#5c7e3e">manifest</span></i></b> file

- To create a <b><span style="color:#d46644">namespace</span></b> declaratively, use the <b><span style="color:#d46644">Namespace</span></b> definition file

![[namespaces-1.png]]

- To create a <b><span style="color:#d46644">namespace</span></b> imperatively, use the <span style="color:red">kubectl create namespace</span> command

![[namespaces-2.png]]

- To get a resource in all <b><span style="color:#d46644">namespaces</span></b>, use the <span style="color:red">--all-namespaces</span> or <span style="color:red">-A</span> option

		kubectl get pods --all-namespaces

- <i><span style="color:#477bbe">Contexts</span></i> are used to manage multiple [[0 - Core Concepts Intro ✅|clusters]] in multiple environments from the same management system

- To limit resources in a <b><span style="color:#d46644">namespace</span></b>, create a <i><span style="color:#477bbe">ResourceQuota</span></i> definition yaml.
	- Add the <b><span style="color:#d46644">namespace</span></b> to the <i><span style="color:#477bbe">metadata</span></i>
	- Then under <i><span style="color:#477bbe">spec</span></i>, provide your limits

![[namespaces-3.png]]