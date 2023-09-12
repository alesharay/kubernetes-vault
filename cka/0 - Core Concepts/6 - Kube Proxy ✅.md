- <b><span style="color:#d46644">Kube-proxy</span></b> is a process that runs on each [[0 - Core Concepts Intro ✅|node]] in the <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]], whose job is to look for new [[10 - Services|services]] and every time a new [[10 - Services|service]] is created, it creates the appropriate rules on each [[0 - Core Concepts Intro ✅|node]] to forward traffic for those [[10 - Services|services]] to the backend [[7 - Pods ✅|pods]].

- One way it does this is by using <i><span style="color:#477bbe">IPTABLES rules</span></i>
	- It creates an <i><span style="color:#477bbe">IPTABLES rule</span></i> on each [[0 - Core Concepts Intro ✅|node]] in the [[0 - Core Concepts Intro ✅|cluster]] and forwards traffic from the <span style="color:#5c7e3e">IP</span> of the [[10 - Services|service]] to the <span style="color:#5c7e3e">IP</span> of the actual [[7 - Pods ✅|pod]] 

- To install <b><span style="color:#d46644">kube-proxy</span></b>, download the binary from the <span style="color:#5c7e3e">Kubernetes</span> release page, extract it, and run it as a [[10 - Services|service]]

- The <b><i><span style="color:#5c7e3e">kubeadm</span></i></b> tool deploys <b><span style="color:#d46644">kube-proxy</span></b> as [[7 - Pods ✅|pods]] on each [[0 - Core Concepts Intro ✅|node]]
	- It is [[9 - Deployments ✅|deployed]] as a [[7 - DaemonSets|daemonSet]], so a single [[7 - Pods ✅|pod]] is always [[9 - Deployments ✅|deployed]] on each [[0 - Core Concepts Intro ✅|node]] in the [[0 - Core Concepts Intro ✅|cluster]]
