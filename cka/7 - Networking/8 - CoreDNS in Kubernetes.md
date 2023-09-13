- REMEMBER: as the [[0 - Core Concepts Intro ✅|cluster]] grows, it is not feasible to add every new [[7 - Pods ✅|pod]] to the <span style="color:red">/etc/hosts</span> file.
	- The better way to do this (if being done manually) is to create a [[0.2 - DNS|DNS server]], and whenever a new [[7 - Pods ✅|pod]] is created, a record in the [[0.2 - DNS|DNS server]] is added for that [[7 - Pods ✅|pod]] so that other [[7 - Pods ✅|pods]] can access the new [[7 - Pods ✅|pod]]
	- You would also configure the <span style="color:red">/etc/resolv.conf</span> file in the [[7 - Pods ✅|pod]] to point to the [[0.2 - DNS|DNS server]] so that the new [[7 - Pods ✅|pod]] can resolve other [[7 - Pods ✅|pods]] in the [[0 - Core Concepts Intro ✅|cluster]]

![[coreDNSk-1.png]]

- <span style="color:#5c7e3e">Kubernetes</span> sort of follows the process of adding [[7 - Pods ✅|pod]] entries to the [[0.2 - DNS|DNS server]], except the [[7 - Pods ✅|pod]] names that are mapped to their <span style="color:#5c7e3e">IP addresses</span> are slightly different

- For [[7 - Pods ✅|pods]], <span style="color:#5c7e3e">Kubernetes</span> forms <i><span style="color:#477bbe">host</span></i> names by replacing dots with dashes in the <span style="color:#5c7e3e">IP address</span> of the [[7 - Pods ✅|pod]]

![[coreDNSk-2.png]]

- Prior to <span style="color:#5c7e3e">Kubernetes</span> version 1.12, the [[0.2 - DNS|DNS server]] implemented by <span style="color:#5c7e3e">Kubernetes</span> was known as [[7 - DNS in Kubernetes|kube-dns]]
	- It is useful to know this for when you are working with legacy systems

- With <span style="color:#5c7e3e">Kubernetes</span> v1.12+, the recommended [[0.2 - DNS|DNS server]] is <b><i><span style="color:#d46644">coreDNS</span></i></b>

### How is CoreDNS setup

- The <b><i><span style="color:#d46644">coreDNS server</span></i></b> is [[9 - Deployments ✅|deployed]] as a [[8 - ReplicaSets ✅|replicaSets]] within a [[9 - Deployments ✅|deployment]] (for redundancy) in the [[11 - Namespaces ✅|kube-system namespace]] in the <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]]

![[coreDNSk-3.png]]

- The <b><i><span style="color:#d46644">coreDNS</span></i></b> [[7 - Pods ✅|pods]] run the <b><i><span style="color:#d46644">coreDNS</span></i></b> executable (the same executable ran when [[9 - Deployments ✅|deploying]] <b><i><span style="color:#d46644">coreDNS</span></i></b> manually)

![[coreDNSk-4.png]]

- <b><i><span style="color:#d46644">coreDNS</span></i></b> requires a configuration file and <span style="color:#5c7e3e">Kubernetes</span> uses a file named <b><i><span style="color:#d46644">Corefile</span></i></b> located at <span style="color:red">/etc/coredns/Corefile</span>

- Within the <b><i><span style="color:#d46644">coreDNS</span></i></b> configuration file (<b><i><span style="color:#d46644">Corefile</span></i></b>), there are a number of <b><i><span style="color:#d46644">plugins</span></i></b> configured (pictured in orange below) which describe how to handle errors, report health, monitor metrics, cache, etc…

![[coreDNSk-5.png]]

- The <b><i><span style="color:#d46644">plugin</span></i></b> that makes <b><i><span style="color:#d46644">coreDNS</span></i></b> work with <span style="color:#5c7e3e">Kubernetes</span> is the <span style="color:#5c7e3e">Kubernetes</span> <b><i><span style="color:#d46644">plugin</span></i></b>
	- This is where the <b><i><span style="color:#d46644">top level domain name</span></i></b> for the [[0 - Core Concepts Intro ✅|cluster]] is configured

![[coreDNSk-6.png]]

- Every record in the <b><i><span style="color:#d46644">coreDNS</span></i></b> [[0.2 - DNS|DNS server]] will fall under the <b><i><span style="color:#d46644">domain</span></i></b> set for the <span style="color:#5c7e3e">Kubernetes</span> <b><i><span style="color:#d46644">plugin</span></i></b> in the <b><i><span style="color:#d46644">Corefile</span></i></b>

- With the <span style="color:#5c7e3e">Kubernetes</span> <b><i><span style="color:#d46644">plugin</span></i></b>, there are multiple options
	- The [[7 - Pods ✅|pod]] option is responsible for creating a record for the [[7 - Pods ✅|pods]] in the [[0 - Core Concepts Intro ✅|cluster]] (this entry is disabled by default)

- Any record that the [[0.2 - DNS|DNS server]] can't solve, it is forwarded to the <b><i><span style="color:#d46644">nameserver</span></i></b> specified in the <b><i><span style="color:#d46644">coreDNS</span></i></b> [[7 - Pods ✅|pod's]] <span style="color:red">/etc/resolv.conf</span> file
	- The <span style="color:red">/etc/resolv.conf</span> file is set the use the <b><i><span style="color:#d46644">nameserver</span></i></b> from the <span style="color:#5c7e3e">Kubernetes</span> node

- The <b><i><span style="color:#d46644">Corefile</span></i></b> is passed into the [[7 - Pods ✅|pod]] as a [[4 - ConfigMaps|configMap]]
	- This way if you need to modify the configuration, you can edit the [[4 - ConfigMaps|configMap]]

- When the <b><i><span style="color:#d46644">coreDNS</span></i></b> [[7 - Pods ✅|pod]] is up and running and watching the <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]] for new [[7 - Pods ✅|pods]] or [[10 - Services ✅|services]], every time a [[7 - Pods ✅|pod]] or [[10 - Services ✅|service]] is created, it adds a record for it

![[coreDNSk-7.png]]

- When the <b><i><span style="color:#d46644">coreDNS</span></i></b> solution is [[9 - Deployments ✅|deployed]], a [[10 - Services ✅|service]] is also created [[10 - Services ✅|services]] made available to other components within a [[0 - Core Concepts Intro ✅|cluster]]

![[coreDNSk-8.png]]

- The <b><i><span style="color:#d46644">coreDNS</span></i></b> [[10 - Services ✅|service]] is named [[7 - DNS in Kubernetes|kube-dns]] by default and the <span style="color:#5c7e3e">IP address</span> of the [[10 - Services ✅|service]]  is configured as the <b><i><span style="color:#d46644">nameserver</span></i></b> on [[7 - Pods ✅|pods]]

![[coreDNSk-9.png]]

- The [[0.2 - DNS|DNS]] configurations on [[7 - Pods ✅|pods]] are done by <span style="color:#5c7e3e">Kubernetes</span> automatically when [[7 - Pods ✅|pods]] are created
	- The [[5 - Kubelet ✅|kubelet]] is responsible for this

- The config file of the [[5 - Kubelet ✅|kubelet]] shows the <span style="color:#5c7e3e">IP address</span> of the [[0.2 - DNS|DNS server]] and domain in it

-  The <span style="color:red">resolv.conf</span> file also has a '<span style="color:red">search</span>' entry that has the following entries so that when you nslookup or search by name, the <b><i><span style="color:#d46644">FQDN</span></i></b> will be used automatically
	- NAMESPACE.svc.cluster.local
	- svc.cluster.local
	- cluster.local

- The <span style="color:red">resolv.conf</span> file only has search entries for [[10 - Services ✅|services]]; for [[7 - Pods ✅|pods]] you must specify the <b><i><span style="color:#d46644">FQDN</span></i></b>