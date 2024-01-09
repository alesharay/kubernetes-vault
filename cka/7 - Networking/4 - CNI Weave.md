- The <b><i><span style="color:#d46644">WeaveWorks weave</span></i></b> <b><i><span style="color:#d46644">CNI plugin</span></i></b> is one of many solutions based on <b><i><span style="color:#d46644">CNI</span></i></b>

- With a <b><i><span style="color:#d46644">network</span></i></b> solution that is manually configured with a [[0.1 - Switching, Routing, Gateways CNI in Kubernetes ✅|routing table]] which maps what <b><i><span style="color:#d46644">networks</span></i></b> are on what <i><span style="color:#477bbe">hosts</span></i>, when a <span style="color:#5c7e3e">packet</span> is sent from one [[7 - Pods ✅|pod]] to another, it goes out to the <b><i><span style="color:#d46644">network</span></i></b>, to the [[0.1 - Switching, Routing, Gateways CNI in Kubernetes ✅|router]] and finds it's way to the [[0 - Core Concepts Intro ✅|node]] that <i><span style="color:#477bbe">hosts</span></i> that [[7 - Pods ✅|pod]]
	- This only works for smaller environments

- When the <b><i><span style="color:#d46644">weave CNI plugin</span></i></b> is deployed on a [[0 - Core Concepts Intro ✅|cluster]], it deploys a service on each [[0 - Core Concepts Intro ✅|node]].
	- These services communicate with each other to exchange information regarding the [[0 - Core Concepts Intro ✅|nodes]], <b><i><span style="color:#d46644">networks</span></i></b> and [[7 - Pods ✅|pods]] within them

![[cni-weave-1.png]]

- Each <b><i><span style="color:#d46644">weave plugin</span></i></b> service (or peer) stores a topology of the entire setup, this way they know what [[7 - Pods ✅|pods]] and their <span style="color:#5c7e3e">IP addresses</span> on all other [[0 - Core Concepts Intro ✅|nodes]]

- <b><i><span style="color:#d46644">Weave</span></i></b> creates it's own [[0.1 - Switching, Routing, Gateways CNI in Kubernetes ✅|bridge]] on the [[0 - Core Concepts Intro ✅|nodes]] and names it <b><i><span style="color:#d46644">weave</span></i></b>

- After the <b><i><span style="color:#d46644">weave plugin</span></i></b> creates the <b><i><span style="color:#d46644">weave bridge</span></i></b> on the [[0 - Core Concepts Intro ✅|node]], it assigns <span style="color:#5c7e3e">IP addresses</span> to each [[0 - Core Concepts Intro ✅|node]] in the  <b><i><span style="color:#d46644">network</span></i></b>

![[cni-weave-2.png]]

- REMEMBER: a single [[7 - Pods ✅|pod]] may be attached to multiple <b><i><span style="color:#d46644">bridge networks</span></i></b>

- The path a <span style="color:#5c7e3e">packet</span> takes to reach it's destination depends on the [[0.1 - Switching, Routing, Gateways CNI in Kubernetes ✅|route]] configured on the [[7 - Pods ✅|container]]

- <b><i><span style="color:#d46644">Weave</span></i></b> makes sure the [[7 - Pods ✅|pods]] get the correct [[0.1 - Switching, Routing, Gateways CNI in Kubernetes ✅|route]] configured to reach the service and the service takes care of the rest

- When a <span style="color:#5c7e3e">packet</span> is sent to a [[7 - Pods ✅|pod]] on another [[0 - Core Concepts Intro ✅|node]], <b><i><span style="color:#d46644">weave</span></i></b> intercepts the <span style="color:#5c7e3e">packet</span> and identifies that it is on a separate <b><i><span style="color:#d46644">network</span></i></b>, encapsulates the <span style="color:#5c7e3e">packet</span> into a new one with new source and destination and sends it across the <b><i><span style="color:#d46644">network</span></i></b>

- Once the <span style="color:#5c7e3e">packet</span> is on the other <b><i><span style="color:#d46644">network</span></i></b>, the other <b><i><span style="color:#d46644">weave</span></i></b> service retrieves the <span style="color:#5c7e3e">packet</span>, decapsulates and [[0.1 - Switching, Routing, Gateways CNI in Kubernetes ✅|routes]] the <span style="color:#5c7e3e">packet</span> to the correct [[7 - Pods ✅|pod]]

- To deploy <b><i><span style="color:#d46644">weave</span></i></b> on a <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]], it can be deployed as services or daemons on each [[0 - Core Concepts Intro ✅|node]] in the [[0 - Core Concepts Intro ✅|cluster]] manually
	- If a [[0 - Core Concepts Intro ✅|cluster]] already exists, the easier way is to deploy as [[7 - Pods ✅|pods]] in the [[0 - Core Concepts Intro ✅|cluster]]

- Once the base <span style="color:#5c7e3e">Kubernetes</span> system is ready with [[0 - Core Concepts Intro ✅|nodes]], <b><i><span style="color:#d46644">networking</span></i></b> is configured correctly between the [[0 - Core Concepts Intro ✅|nodes]], and the basic [[0 - Core Concepts Intro ✅|controlplane components]] are deployed, <b><i><span style="color:#d46644">weave</span></i></b> can be [[9 - Deployments ✅|deployed]] in the [[0 - Core Concepts Intro ✅|cluster]] with a simple command:

`kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')`

- When <b><i><span style="color:#d46644">weave</span></i></b> is applied, it deploys all the necessary components required for <b><i><span style="color:#d46644">weave</span></i></b> in the [[0 - Core Concepts Intro ✅|cluster]]

- The <b><i><span style="color:#d46644">weave</span></i></b> peers are [[9 - Deployments ✅|deployed]] as a [[7 - DaemonSets ✅|daemonsets]] which ensures that one <b><i><span style="color:#d46644">weave</span></i></b> [[7 - Pods ✅|pod]]  is [[9 - Deployments ✅|deployed]] on each [[0 - Core Concepts Intro ✅|node]] in the [[0 - Core Concepts Intro ✅|cluster]]