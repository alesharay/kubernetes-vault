- [[7 - Pods ✅|Pods]] are rarely configured to communicate with each other directly

- If a [[7 - Pods ✅|pod]] should be able to access [[10 - Services ✅|services]] on another [[7 - Pods ✅|pod]], it would use a [[10 - Services ✅|service]]

- When a [[10 - Services ✅|service]] is created, it is accessible from all [[7 - Pods ✅|pods]] on the [[0 - Core Concepts Intro ✅|cluster]] regardless of what [[0 - Core Concepts Intro ✅|nodes]] the [[7 - Pods ✅|pods]] are on

- While a [[7 - Pods ✅|pod]] is <i><span style="color:#477bbe">hosted</span></i> on a [[0 - Core Concepts Intro ✅|node]], a [[10 - Services ✅|service]] is <i><span style="color:#477bbe">hosted</span></i> across a [[0 - Core Concepts Intro ✅|cluster]] and is not [[3 - Volumes ✅|bound]] to a specific [[0 - Core Concepts Intro ✅|node]]

- ***REMEMBER***: A [[10 - Services ✅|service]] that is only accessible from within the [[0 - Core Concepts Intro ✅|cluster]] is known as [[10 - Services ✅|clusterIP]]

- ***REMEMBER***: If a [[10 - Services ✅|service]] needs to be accessible outside of the [[0 - Core Concepts Intro ✅|cluster]], a [[10 - Services ✅|NodePort]] [[10 - Services ✅|service]] is created

- With a [[10 - Services ✅|NodePort]] [[10 - Services ✅|service]], in addition to it getting an <span style="color:#5c7e3e">IP address</span> that the [[7 - Pods ✅|pods]] can interact with (like any other [[10 - Services ✅|service]]), it also exposes a <span style="color:#5c7e3e">port</span> on all [[0 - Core Concepts Intro ✅|nodes]] in the [[0 - Core Concepts Intro ✅|cluster]]; this way, external users or applications can access this [[10 - Services ✅|service]]

- The focus of this section answers the questions:
	- How are the [[10 - Services ✅|services]] getting these <span style="color:#5c7e3e">IP addresses</span>
	- How are the <b><i><span style="color:#d46644">service IP address</span></i></b> made available across all of the [[0 - Core Concepts Intro ✅|nodes]] in the [[0 - Core Concepts Intro ✅|cluster]]
	- How are the [[10 - Services ✅|services]] made available to external users through a <span style="color:#5c7e3e">port</span> on each [[0 - Core Concepts Intro ✅|node]]
	- Who is doing these things and how/where can we see it

### Process (example)

- Starting with a blank 3 [[0 - Core Concepts Intro ✅|node]] [[0 - Core Concepts Intro ✅|cluster]], we know that every <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|node]] runs a [[5 - Kubelet ✅|kubelet]] process, which is responsible for creating the [[7 - Pods ✅|pods]]

![[service-networking-1.png]]

- Each [[5 - Kubelet ✅|kubelet]] [[10 - Services ✅|service]] watches for changes in the [[0 - Core Concepts Intro ✅|cluster]] through the [[2 - Kube API server ✅|kube-apiserver]] and every time a new [[7 - Pods ✅|pod]] is instructed to be created, [[5 - Kubelet ✅|kubelet]] creates the [[7 - Pods ✅|pod]] on a [[0 - Core Concepts Intro ✅|node]]
	- [[5 - Kubelet ✅|kubelet]], then, invokes the [[0.6 - CNI|CNI plugin]] to configure <b><i><span style="color:#d46644">networking</span></i></b> for that [[7 - Pods ✅|pod]]

![[service-networking-2.png]]

- Similarly, each [[0 - Core Concepts Intro ✅|node]] runs a process known as [[6 - Kube Proxy ✅|kube-proxy]]

![[service-networking-3.png]]

- [[6 - Kube Proxy ✅|Kube-proxy]] watches for changes through the [[2 - Kube API server ✅|kube-apiserver]] and every time a new [[10 - Services ✅|service]] is to be created, [[6 - Kube Proxy ✅|kube-proxy]] goes to work; and unlike [[7 - Pods ✅|pods]], [[10 - Services ✅|services]] are not created on each [[0 - Core Concepts Intro ✅|node]] and are instead a [[0 - Core Concepts Intro ✅|cluster-wide]] concept

![[service-networking-4.png]]

- As we have seen, [[7 - Pods ✅|pods]] have [[7 - Pods ✅|containers]] and [[7 - Pods ✅|containers]] have [[0.4 - Network Namespaces ✅|namespaces]] with [[0.6 - CNI|interfaces]] that have <span style="color:#5c7e3e">IP addresses</span> assigned to them. But with [[10 - Services ✅|services]], nothing like this exists
	- There are no processes, [[0.4 - Network Namespaces ✅|namespaces]] or [[0.6 - CNI|interfaces]] for [[10 - Services ✅|services]]

- A [[10 - Services ✅|service]] is just a <span style="color:#5c7e3e">virtual</span> object

- If a [[10 - Services ✅|service]] is just a <span style="color:#5c7e3e">virtual</span> object, then how does it get an <span style="color:#5c7e3e">IP address</span> and how are they accessible to [[7 - Pods ✅|pods]]?

- When a [[10 - Services ✅|service]] is created in <span style="color:#5c7e3e">Kubernetes</span>, it is assigned an <span style="color:#5c7e3e">IP address</span> from a predefined <b><i><span style="color:#d46644">range</span></i></b>

- The [[6 - Kube Proxy ✅|kube-proxy]] components running on each [[0 - Core Concepts Intro ✅|node]] gets the <b><i><span style="color:#d46644">service's</span></i></b> <span style="color:#5c7e3e">IP address</span> and creates forwarding rules on each [[0 - Core Concepts Intro ✅|node]] in the [[0 - Core Concepts Intro ✅|cluster]] which states that any traffic coming to the <span style="color:#5c7e3e">IP</span> of the [[10 - Services ✅|service]] should go to the <span style="color:#5c7e3e">IP</span> of the [[7 - Pods ✅|pod]]

![[service-networking-5.png]]

- Once the <b><i><span style="color:#d46644">service's IP</span></i></b> forwarding is in place, whenever a [[7 - Pods ✅|pod]] tries to reach the <span style="color:#5c7e3e">IP</span> of a [[10 - Services ✅|service]], it is forwarded to the destination [[7 - Pods ✅|pod's]] <span style="color:#5c7e3e">IP</span> which is accessible from any [[0 - Core Concepts Intro ✅|node]] in the [[0 - Core Concepts Intro ✅|cluster]]
	- Remember, it is not just an <span style="color:#5c7e3e">IP</span>, it is an <span style="color:#5c7e3e">IP</span> and <span style="color:#5c7e3e">port</span> combination

![[service-networking-6.png]]

- Whenever a [[10 - Services ✅|service]] is created or deleted, [[6 - Kube Proxy ✅|kube-proxy]] creates or deletes the forwarding rules

- [[6 - Kube Proxy ✅|Kube-proxy]] can create forwarding rules in many different ways but the default (and most common) is by using <span style="color:#5c7e3e">IPTables</span> rules

![[service-networking-7.png]]

- The proxy mode can be set using the <span style="color:red">--proxy-mode</span> option when configuring the [[6 - Kube Proxy ✅|kube-proxy]] [[10 - Services ✅|service]] and if this is not set, it will default to <span style="color:#5c7e3e">IPTables</span>

![[service-networking-8.png]]

- When a [[10 - Services ✅|service]] is created and an <span style="color:#5c7e3e">IP address</span> is assigned to it, the option in the [[2 - Kube API server ✅|kube-apiserver]] called "<span style="color:red">--service-cluster-ip-range</span>" is set
	- By default, this is set to <span style="color:red">10.0.0.0/24</span>

![[service-networking-9.png]]

- You can see what it is set to by running <span style="color:red">ps aux | grep kube-api-server</span>

![[service-networking-10.png]]

- The <span style="color:#5c7e3e">IP address</span> <b><i><span style="color:#d46644">ranges</span></i></b> specified for each network (i.e. [[7 - Pods ✅|pod]], [[10 - Services ✅|service]], etc…) should not overlap
	- When [[7 - Pods ✅|pod]] <b><i><span style="color:#d46644">networking</span></i></b> is setup, a <b><i><span style="color:#d46644">CIDR range</span></i></b> is provided which means all [[7 - Pods ✅|pod]] <span style="color:#5c7e3e">IP addresses</span> will fall within that <b><i><span style="color:#d46644">range</span></i></b>. This <b><i><span style="color:#d46644">range</span></i></b> should not be the same as the [[10 - Services ✅|service]] <b><i><span style="color:#d46644">range</span></i></b>

- The rules created by the [[6 - Kube Proxy ✅|kube-proxy]] can be seen in the <span style="color:#5c7e3e">IPTables NAT table</span> output
	- You can search for the name of the [[10 - Services ✅|service]] as all rules created by [[6 - Kube Proxy ✅|kube-proxy]] have a comment with the name of the [[10 - Services ✅|service]] on it

![[service-networking-11.png]]

- Using the given example, this means, any traffic going to <span style="color:#5c7e3e">IP address</span> 10.103.132.104 <span style="color:#5c7e3e">port</span> 3306 (the [[10 - Services ✅|service]] <span style="color:#5c7e3e">IP address</span>),  should have its <span style="color:#5c7e3e">IP address</span> changed to 10.244.1.2 <span style="color:#5c7e3e">port</span> 3306 (the [[7 - Pods ✅|pod]] <span style="color:#5c7e3e">IP address</span>)
	- This is done by adding a <span style="color:#5c7e3e">DNAT</span> rule to <span style="color:#5c7e3e">IPTables</span>

![[service-networking-12.png]]

- You can watch [[6 - Kube Proxy ✅|kube-proxy]] create the <span style="color:#5c7e3e">IPTables</span> entries in the [[6 - Kube Proxy ✅|kube-proxy]] logs which will show what '<b><i><span style="color:#d46644">proxier</span></i></b>' is used (userspace, <span style="color:#5c7e3e">IPTables</span>, etc…), and the entry added when a new [[10 - Services ✅|service]] is added
	- The location of this file might vary depending on your installation

![[service-networking-13.png]]
