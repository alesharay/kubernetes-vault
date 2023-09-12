### Basics of traffic flow and rules

- There are many configurations for an application. Assume you have a <i><span style="color:#477bbe">web server</span></i> serving <i><span style="color:#477bbe">frontend</span></i> to <i><span style="color:#477bbe">users</span></i> and an <i><span style="color:#477bbe">app server</span></i> serving <i><span style="color:#477bbe">backend API server</span></i> and <i><span style="color:#477bbe">database server</span></i>

![[np-1.png]]

- Consider the following scenario: A <i><span style="color:#477bbe">user</span></i> sends a request to the <i><span style="color:#477bbe">web server</span></i> , the <i><span style="color:#477bbe">web server</span></i> then sends a request to the [[2 - Kube API server ✅|API server]]  in the <i><span style="color:#477bbe">backend</span></i>, the [[2 - Kube API server ✅|API server]] then fetches data from the <i><span style="color:#477bbe">database server</span></i> and then sends the data back to the <i><span style="color:#477bbe">user</span></i>

![[np-2.png]]

- There are two types of traffic in the <i><span style="color:#477bbe">web server</span></i> - [[2 - Kube API server ✅|API server]] - <i><span style="color:#477bbe">DB server</span></i> setup: [[9 - Ingress|ingress]] and [[9 - Ingress|egress]]

- For a <i><span style="color:#477bbe">web server</span></i>, the traffic coming in from <i><span style="color:#477bbe">users</span></i> is an [[9 - Ingress|ingress traffic]] and the outgoing requests from the <i><span style="color:#477bbe">web server</span></i> to the [[2 - Kube API server ✅|API server]] is an [[9 - Ingress|egress traffic]]

![[np-3.png]]

- When you define [[9 - Ingress|ingress]] and [[9 - Ingress|egress]], remember you're only looking at the direction in which the traffic originated
	- The response back to the <i><span style="color:#477bbe">user</span></i> doesn't really matter

- For an [[2 - Kube API server ✅|API server]], it receives [[9 - Ingress|ingress traffic]] from the <i><span style="color:#477bbe">web server</span></i> and sends [[9 - Ingress|egress traffic]] to the <i><span style="color:#477bbe">DB server</span></i>
	- The <i><span style="color:#477bbe">DB server</span></i> receives [[9 - Ingress|ingress traffic]] from the [[2 - Kube API server ✅|API server]]

![[np-4.png]]

- If required <b><i><span style="color:#d46644">rules</span></i></b> to get the <i><span style="color:#477bbe">web server</span></i> - [[2 - Kube API server ✅|API server]] - <i><span style="color:#477bbe">DB server</span></i> setup working were listed, we would have:
	- an [[9 - Ingress|ingress rule]] that is required to accept HTTP traffic
	- an [[9 - Ingress|egress rule]] to allow traffic from the <i><span style="color:#477bbe">web server</span></i> to the [[2 - Kube API server ✅|API server]]
	- an [[9 - Ingress|ingress rule]] to accept traffic on the [[2 - Kube API server ✅|API server]]
	- an [[9 - Ingress|egress rule]] to allow traffic on the <i><span style="color:#477bbe">DB server</span></i>
	- an [[9 - Ingress|ingress rule]] to accept traffic on the <i><span style="color:#477bbe">DB server</span></i>

![[np-5.png]]

### Network security in Kubernetes

- On a [[0 - Core Concepts Intro ✅|cluster]] with a set of [[0 - Core Concepts Intro ✅|nodes]] hosting a set of [[7 - Pods|pods]] and [[10 - Services|services]], each [[0 - Core Concepts Intro ✅|node]], [[7 - Pods|pod]], and [[10 - Services|services]] have their own IP address

![[np-6.png]]

- One of the prerequisites for <b><i><span style="color:#d46644">networking</span></i></b> in <span style="color:#5c7e3e">Kubernetes</span> is that with whatever solution you implement, the [[7 - Pods|pods]] should be able to communicate with each other without having to configure any additional settings like routes.

- In <b><i><span style="color:#d46644">networking</span></i></b>, all [[7 - Pods|pods]] should, by default, be able to reach each other by their IP addresses, [[7 - Pods|pod]] names, or [[10 - Services|services]]

- *Remember: <span style="color:#5c7e3e">Kubernetes</span> is configured, by default, with an "<b><i><span style="color:#d46644">Allow All</span></i></b>" <b><i><span style="color:#d46644">rule</span></i></b> that allows traffic from any [[7 - Pods|pod]] to any other [[7 - Pods|pod]] or [[10 - Services|service]]

- Using the <i><span style="color:#477bbe">web server</span></i> - [[2 - Kube API server ✅|API server]] - <i><span style="color:#477bbe">DB server</span></i> setup but in <span style="color:#5c7e3e">Kubernetes</span>, for each component in the application, we deploy a [[7 - Pods|pod]]: one for the <i><span style="color:#477bbe">web server</span></i>, one for the [[2 - Kube API server ✅|API server]], and one for the <i><span style="color:#477bbe">DB server</span></i>

- Create [[10 - Services|services]] to enable communication between the [[7 - Pods|pods]] as well as the <i><span style="color:#477bbe">end users</span></i>

- If we don't want the <i><span style="color:#477bbe">frontend web server</span></i> to be able to communicate with the <i><span style="color:#477bbe">DB server</span></i> directly, you would implement a <b><i><span style="color:#d46644">network policy</span></i></b> to allow traffic to the <i><span style="color:#477bbe">DB server</span></i> only from the [[2 - Kube API server ✅|API server]]

- A <b><i><span style="color:#d46644">network policy</span></i></b> is another object in the <span style="color:#5c7e3e">Kubernetes</span> [[11 - Namespaces|namespace]]

- You link a <b><i><span style="color:#d46644">network policy</span></i></b> to one ore more [[7 - Pods|pods]]

![[np-7.png]]

- You can define <b><i><span style="color:#d46644">rules</span></i></b> within the <b><i><span style="color:#d46644">network policy</span></i></b>

- Once the <b><i><span style="color:#d46644">network policy</span></i></b> is created, it blocks all other traffic to the [[7 - Pods|pod]] and only allows traffic that matches the specified <b><i><span style="color:#d46644">rule</span></i></b>
	- This is only applicable to the [[7 - Pods|pod]] on which the <b><i><span style="color:#d46644">network policy</span></i></b> is applied

- To apply or link a <b><i><span style="color:#d46644">network policy</span></i></b> to a [[7 - Pods|pod]], use labels and selectors

- To apply a <b><i><span style="color:#d46644">network policy</span></i></b>, label the [[7 - Pods|pod]] and apply the same [[1 - Labels & Selectors|labels]] on the <span style="color:red">podSelector</span> field in the <b><i><span style="color:#d46644">network policy</span></i></b>

![[np-8.png]]

- After the [[7 - Pods|pod]] is [[1 - Labels & Selectors|labeled]] and the <b><i><span style="color:#d46644">rule</span></i></b> is built, under <b><i><span style="color:#d46644">policy types</span></i></b> we specify whether the <b><i><span style="color:#d46644">rule</span></i></b> is to allow [[9 - Ingress|ingress]] or [[9 - Ingress|egress traffic]] or both

![[np-9.png]]

- Specify the [[9 - Ingress|ingress rule]] that allows traffic from the sending [[7 - Pods|pods]] to the receiving [[7 - Pods|pod]] using [[1 - Labels & Selectors|labels and selectors]].
	- Then add the port of the sending [[7 - Pods|pod]]

![[np-10.png]]

- The <span style="color:#5c7e3e">apiVersion</span> for a <b><i><span style="color:#d46644">network policy</span></i></b> object is networking.k8s.io/v1. The <span style="color:#5c7e3e">kind</span> is <b><i><span style="color:#d46644">NetworkPolicy</span></i></b>

- In a <b><i><span style="color:#d46644">network policy</span></i></b> object file, under the <span style="color:#5c7e3e">spec</span> section,  add the [[1 - Labels & Selectors|podSelector]] to apply the <b><i><span style="color:#d46644">policy</span></i></b>, then the <b><i><span style="color:#d46644">rule</span></i></b>

![[np-11.png]]

- *Remember: <b><i><span style="color:#d46644">network policies</span></i></b> are enforced by the <b><i><span style="color:#d46644">network</span></i></b> solution implemented on the <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]]

- Not all solutions support <b><i><span style="color:#d46644">network policies</span></i></b>; a few of them that are supported are kube-router, Calico, Romana and Weave-net
	- Flannel does not

![[np-12.png]]

- Always refer to the <b><i><span style="color:#d46644">network</span></i></b> solution's documentation to see support for <b><i><span style="color:#d46644">network policies</span></i></b>

- Even in a [[0 - Core Concepts Intro ✅|cluster]] configured with a solution that does not support <b><i><span style="color:#d46644">network policies</span></i></b>, you can still create the <b><i><span style="color:#d46644">policies</span></i></b> but they will just not be enforced
	- You will not get an error message saying that the <b><i><span style="color:#d46644">network</span></i></b> solution does not support <b><i><span style="color:#d46644">network policies</span></i></b>