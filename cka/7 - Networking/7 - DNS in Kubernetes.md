- <span style="color:#5c7e3e">Kubernetes</span> [[9 - Deployments|deploys]] a built-in <b><i><span style="color:#d46644">DNS server</span></i></b> by default when a [[0 - Core Concepts Intro|cluster]] is created
	- If <span style="color:#5c7e3e">Kubernetes</span> is setup manually, the <b><i><span style="color:#d46644">DNS server</span></i></b> is also setup manually

![[dnsk-1.png]]

- As far as <b><i><span style="color:#d46644">DNS</span></i></b> is concerned, it is assumed that all [[7 - Pods|pods]] and [[10 - Services|services]] can reach each other using their <span style="color:#5c7e3e">IP addresses</span>

- Whenever a [[10 - Services|service]] is created, the <span style="color:#5c7e3e">Kubernetes</span> <b><i><span style="color:#d46644">DNS</span></i></b> [[10 - Services|service]] creates a record for it which maps the name of the [[10 - Services|service]] to its <span style="color:#5c7e3e">IP address</span>

![[dnsk-2.png]]

- Once the [[10 - Services|service]] name is mapped to its <span style="color:#5c7e3e">IP address</span>, any [[7 - Pods|pod]] in the [[0 - Core Concepts Intro|cluster]] can use or reach the [[10 - Services|service]] by name

![[dnsk-3.png]]

- If the [[7 - Pods|pod]] and the [[10 - Services|service]] are in the same [[11 - Namespaces|namespace]], the [[7 - Pods|pod]] can access the [[10 - Services|service]] by just it's name. But if they are in different [[11 - Namespaces|namespaces]], the [[7 - Pods|pod]] must use the [[10 - Services|services]] full name for access (where the last portion of the name is the [[11 - Namespaces|namespace]])

![[dnsk-4.png]]

- For each [[11 - Namespaces|namespace]], the <b><i><span style="color:#d46644">DNS</span></i></b> [[10 - Services|service]] creates a <b><i><span style="color:#d46644">subdomain</span></i></b>

- All [[10 - Services|services]] are group together in another <b><i><span style="color:#d46644">subdomain</span></i></b> known as [[10 - Services|svc]]

- To reiterate: all [[7 - Pods|pods]] and [[10 - Services|services]] in the same [[11 - Namespaces|namespace]] are grouped together within a <b><i><span style="color:#d46644">subdomain</span></i></b> with same name as the [[11 - Namespaces|namespace]] and ALL [[10 - Services|services]] are also grouped together in another <b><i><span style="color:#d46644">subdomain</span></i></b> called [[10 - Services|svc]]

![[dnsk-5.png]]

- In addition to the previously mentioned groupings, all [[10 - Services|services]] and [[7 - Pods|pods]] are grouped together into a <b><i><span style="color:#d46644">root domain</span></i></b> for the [[0 - Core Concepts Intro|cluster]], which is set to <span style="color:red">cluster.local</span> by default
	- So you can access the [[10 - Services|service]] using the URL <span style="color:red">SERVICE_NAME.NAMESPACE_NAME.svc.cluster.local</span>
	- This is the <b><i><span style="color:#d46644">fully qualified domain name</span></i></b> (<b><i><span style="color:#d46644">FQDN</span></i></b>) for the [[10 - Services|service]]

![[dnsk-6.png]]

- <b><i><span style="color:#d46644">DNS</span></i></b> records for [[7 - Pods|pods]] are not created by default but can be enabled explicitly
	- Once explicitly set, records will be created for [[7 - Pods|pods]] as well
	- For each [[7 - Pods|pod]], <span style="color:#5c7e3e">Kubernetes</span> generates a name by:
		- Replacing the dots in the [[7 - Pods|pod]] <span style="color:#5c7e3e">IP address</span> with dashes
		- The [[11 - Namespaces|namespace]] is the same as whichever one the [[7 - Pods|pod]] is in
		- The type <b><i><span style="color:#d46644">subdomain</span></i></b> becomes [[7 - Pods|pod]]
		- The <b><i><span style="color:#d46644">root domain</span></i></b> is always <span style="color:red">cluster.local</span>

![[dnsk-7.png]]