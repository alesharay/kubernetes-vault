- Assume the goal is to protect the <i><span style="color:#477bbe">DB</span></i> [[7 - Pods ✅|pod]] in the <i><span style="color:#477bbe">web server</span></i> - <i><span style="color:#477bbe">API server</span></i>- <i><span style="color:#477bbe">DB server</span></i> setup so that it doesn't allow access from any other [[7 - Pods ✅|pods]] except the <i><span style="color:#477bbe">API server</span></i> [[7 - Pods ✅|pod]]

- By default, <span style="color:#5c7e3e">Kubernetes</span> allows all communications from all [[7 - Pods ✅|pods]] to all destinations

- Consider the <i><span style="color:#477bbe">web server</span></i> - <i><span style="color:#477bbe">API server</span></i> - <i><span style="color:#477bbe">DB server</span></i> setup: as a first step, we want to block out everything going in and out of the <i><span style="color:#477bbe">DB</span></i> [[7 - Pods ✅|pod]]

- Protect the pod by creating a [[14 - Network Policies ✅|NetworkPolicy]] object
	- Give it a name that represents what it's for

- *Remember: <b><i><span style="color:#d46644">network policies</span></i></b> are created using [[1 - Labels & Selectors ✅|labels and selectors]]

- To add [[1 - Labels & Selectors ✅|labels and selectors]] to the [[14 - Network Policies ✅|NetworkPolicy]] object, add a [[1 - Labels & Selectors ✅|podSelector]] property with a [[1 - Labels & Selectors ✅|matchLabel]] field under it specifying the [[1 - Labels & Selectors ✅|label]] on the [[7 - Pods ✅|pod]]
	- This associates the <b><i><span style="color:#d46644">network policy</span></i></b> with the [[7 - Pods ✅|pod]] and blocks out all traffic

![[developing-1.png]]

- After blocking out all traffic to the [[7 - Pods ✅|pod]], we need to choose which [[7 - Pods ✅|pod]] needs to be able to access the [[7 - Pods ✅|pod]] and on which <span style="color:#5c7e3e">port</span>

- It is necessary to decide what type of [[14 - Network Policies ✅|policies]] should be defined on the <b><i><span style="color:#d46644">network policy</span></i></b> object to meet necessary requirements
	- [[14 - Network Policies ✅|ingress]], [[14 - Network Policies ✅|egress]] or both

- From the <i><span style="color:#477bbe">DB</span></i> [[7 - Pods ✅|pod]] perspective, we want to allow traffic from the <i><span style="color:#477bbe">API server</span></i> [[7 - Pods ✅|pod]]
	- This is incoming traffic so we need an [[14 - Network Policies ✅|ingress]] <b><i><span style="color:#d46644">network policy</span></i></b>

![[developing-2.png]]

- Once you allow incoming traffic, the response or reply to that traffic is allowed back automatically
	- Thus a separate <span style="color:#5c7e3e">rule</span> is not needed

- When deciding on what type of <span style="color:#5c7e3e">rule</span> needs to be created, you only need to be concerned about the direction in which the requests originate
	- This is denoted by the straight line in the [[11 - Image Security ✅|image]]

![[developing-3.png]]

- For an [[14 - Network Policies ✅|ingress policy]], even though response traffic is still allowed through, new outgoing traffic (like a call from the <i><span style="color:#477bbe">DB</span></i> to the <i><span style="color:#477bbe">API</span></i>) can't

- A single <b><i><span style="color:#d46644">network policy</span></i></b> can have an [[14 - Network Policies ✅|ingress type]], and [[14 - Network Policies ✅|egress type]], or both in the case where a [[7 - Pods ✅|pod]] wants to allow incoming connections as well as wants to make external calls (like an <i><span style="color:#477bbe">API</span></i>)

- Once you decide on the [[14 - Network Policies ✅|policy type]], the next step is to define the specifics of that policy

- If the type is [[14 - Network Policies ✅|ingress]], create a section called [[14 - Network Policies ✅|ingress]] in which we specify multiple <span style="color:#5c7e3e">rules</span>

- Each <span style="color:#5c7e3e">rule</span> for a [[14 - Network Policies ✅|policy type]] has a <span style="color:#5c7e3e">from</span> and <span style="color:#5c7e3e">ports</span> property

- The from property defines the source of traffic that is allowed to pass through to the specified [[7 - Pods ✅|pod]]

- For the from property, we use a [[1 - Labels & Selectors ✅|podSelector]] and provide the [[1 - Labels & Selectors ✅|labels]] of the <i><span style="color:#477bbe">API</span></i> [[7 - Pods ✅|pod]]

![[developing-4.png]]

- The <span style="color:#5c7e3e">ports</span> property defines what <span style="color:#5c7e3e">port</span> on the [[7 - Pods ✅|pod]] the traffic is allowed to go to

![[developing-5.png]]

- If there are multiple [[7 - Pods ✅|pods]] (ie. In different [[11 - Namespaces ✅|namespaces]]) that need access to the specified [[7 - Pods ✅|pod]], the created <b><i><span style="color:#d46644">network policy</span></i></b> allows any [[7 - Pods ✅|pod]] in any [[11 - Namespaces ✅|namespace]] with [[1 - Labels & Selectors ✅|matchLabels]] to reach that [[7 - Pods ✅|pod]]

- If we only want to allow a [[7 - Pods ✅|pod]] in a specific [[11 - Namespaces ✅|namespace]] to reach the specified [[7 - Pods ✅|pod]], even though other [[7 - Pods ✅|pods]] have matching [[1 - Labels & Selectors ✅|labels]], we add a new selector called the [[1 - Labels & Selectors ✅|namespaceSelector]] on the same <span style="color:#5c7e3e">rule</span> as the [[1 - Labels & Selectors ✅|podSelector]]
	- Here we match [[1 - Labels & Selectors ✅|labels]] again using the [[11 - Namespaces ✅|namespace]] [[1 - Labels & Selectors ✅|label]] which should be specified on the [[11 - Namespaces ✅|namespace]] first

![[developing-6.png]]

- The [[1 - Labels & Selectors ✅|namespaceSelector]] helps in defining from what [[11 - Namespaces ✅|namespace]] traffic is allowed to reach the specified [[7 - Pods ✅|pod]]

- If you only have the [[1 - Labels & Selectors ✅|namespaceSelector]] and not the [[1 - Labels & Selectors ✅|podSelector]], all [[7 - Pods ✅|pods]] within the specified [[11 - Namespaces ✅|namespace]] will be allowed to reach the specified [[7 - Pods ✅|pod]]
	- [[7 - Pods ✅|Pods]] from outside of this [[11 - Namespaces ✅|namespace]] will not be allowed to go through

- If we have a <i><span style="color:#477bbe">server</span></i>, that is not apart of the <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]], that we want to be able to access the specified [[7 - Pods ✅|pod]], the current [[1 - Labels & Selectors ✅|podSelector]] and [[11 - Namespaces ✅|namespaceSelector]] won't work

- It is possible to configure <b><i><span style="color:#d46644">network policies</span></i></b> to allow traffic from a specific IP address by adding an <span style="color:#5c7e3e">ipBlock</span> property.
	- This will have the CIDR property under it with the IP CIDR

![[developing-7.png]]

- The [[1 - Labels & Selectors ✅|podSelector]], [[1 - Labels & Selectors ✅|namespaceSelector]], and <span style="color:#5c7e3e">ipBlock</span> properties are all able to be used in both the <span style="color:#5c7e3e">from</span> and <span style="color:#5c7e3e">to</span> sections of the <b><i><span style="color:#d46644">network policy</span></i></b> object

- [[1 - Labels & Selectors ✅|podSelectors]] select [[7 - Pods ✅|pods]] by [[1 - Labels & Selectors ✅|labels]]

- [[1 - Labels & Selectors ✅|namespaceSelectors]] select [[11 - Namespaces ✅|namespaces]] by [[1 - Labels & Selectors ✅|labels]]

- <span style="color:#5c7e3e">ipBlock</span> [[1 - Labels & Selectors ✅|selectors]] select IP address ranges

- The [[1 - Labels & Selectors ✅|podSelector]], [[1 - Labels & Selectors ✅|namespaceSelector]], and <span style="color:#5c7e3e">ipBlock</span> properties can be passed in separately or together as part of a single <span style="color:#5c7e3e">rule</span>

- The multiple <span style="color:#5c7e3e">rules</span> work like OR operators thus traffic for meeting any of these criteria will be allowed to pass through

![[developing-8.png]]

- When there are multiple [[1 - Labels & Selectors ✅|selectors]] in the same <span style="color:#5c7e3e">rule</span>, all traffic must meet the requirements of all selectors in the <span style="color:#5c7e3e">rule</span>, like an AND operation

![[developing-9.png]]

- It's important to understand the possibilities of <span style="color:#5c7e3e">rule</span> configuration

- There can be both [[14 - Network Policies ✅|ingress]] and [[14 - Network Policies ✅|egress]] types on the same <b><i><span style="color:#d46644">network policy</span></i></b>

- Just like with [[14 - Network Policies ✅|ingress]], [[14 - Network Policies ✅|egress]] needs to be added to the <span style="color:#5c7e3e">policyTypes</span> section and there needs to be an [[14 - Network Policies ✅|egress]] section

- To define the specifics of the [[14 - Network Policies ✅|egress policy]] type, and the to (instead of from) and <span style="color:#5c7e3e">ports</span> properties
	- In the <span style="color:#5c7e3e">ports</span> section, specify the <span style="color:#5c7e3e">port</span> for which to send traffic

![[developing-10.png]]

### Practice Problems

- How many network policies do you see in the environment?

		kubectl get netpol

	(jot down the name of the network policy)

- Which pod is the network policy applied on?

		kubectl describe  netpol <name of previous network policy>

	(jot down the name of the podSelector matchLabel)

- What type of traffic is Network Policy configured to handle?

		kubectl describe  netpol <name of previous network policy>

	(jot down the policy type)