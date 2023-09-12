- <i><span style="color:#477bbe">Admins</span></i> and other <i><span style="color:#477bbe">users</span></i> of a [[0 - Core Concepts Intro ✅|cluster]] (developers, testers, etc…) are able to perform all types of operations in the [[0 - Core Concepts Intro ✅|cluster]]; thus <b><i><span style="color:#d46644">authorization</span></i></b> helps limit how much power a particular <i><span style="color:#477bbe">user</span></i> has
	- Ex. A developer shouldn't have the same access as an <i><span style="color:#477bbe">admin</span></i> (ie. the ability to modify the [[5 - Kubeconfig|kubeconfig]] file)

- There are different <b><i><span style="color:#d46644">authorization</span></i></b> mechanisms supported by <span style="color:#5c7e3e">Kubernetes</span>
	- Node Auth
	- Attribute Based Auth (ABAC)
	- Role Based Auth (RBAC)
	- Webhook Auth
	- * Always Allow
	- * Always Deny

- The requests that [[5 - Kubelet|kubelet]] makes to/from the [[2 - Kube API server ✅|kube-apiserver]] are made by a special handler known as the [[0 - Core Concepts Intro ✅|node]] <b><i><span style="color:#d46644">authorizer</span></i></b>
	- This is because [[5 - Kubelet|kubelet]] is a [[0 - Core Concepts Intro ✅|node]]

- Any request coming from a <i><span style="color:#477bbe">user</span></i> with the name <b><i><span style="color:#d46644">system:node</span></i></b> and is part of the <b><i><span style="color:#d46644">system:nodes</span></i></b> <i><span style="color:#477bbe">group</span></i> is <b><i><span style="color:#d46644">authorized</span></i></b> by the [[0 - Core Concepts Intro ✅|node]] <b><i><span style="color:#d46644">authorizer</span></i></b> and are granted those privileges
	- Such as the privileges required by [[5 - Kubelet|kubelet]] (this is access within the [[0 - Core Concepts Intro ✅|cluster]])

- <b><i><span style="color:#d46644">Attribute Based Authorization</span></i></b> is where you associate a <i><span style="color:#477bbe">user</span></i> or <i><span style="color:#477bbe">group</span></i> of <i><span style="color:#477bbe">users</span></i> with a set of permissions
	- Ie.. The dev <i><span style="color:#477bbe">user</span></i> can view, create, and delete [[7 - Pods|pods]]
	- (This is external access to the API)        

- To create <b><i><span style="color:#d46644">Attribute Based Authorization</span></i></b>, you use a set of policy files with the policies defined in <span style="color:#5c7e3e">JSON</span> format
	- This file is then passed into the [[2 - Kube API server ✅|API server]]

![[authorization-1.png]]

- With <b><i><span style="color:#d46644">Attribute Based Authorization</span></i></b>, a policy definition file is created for each <i><span style="color:#477bbe">user</span></i> or <i><span style="color:#477bbe">group</span></i>

- With <b><i><span style="color:#d46644">Attribute Based Authorization</span></i></b>, every time a change needs to be to the security, the policy definition file must be edited manually and the [[2 - Kube API server ✅|API server]] restarted

![[authorization-2.png]]

- Because the <b><i><span style="color:#d46644">Attribute Based Access Control</span></i></b> (<b><i><span style="color:#d46644">ABAC</span></i></b>) files have to be modified manually and the [[2 - Kube API server ✅|API server]] restarted when a change needs to be made, <b><i><span style="color:#d46644">ABAC</span></i></b> configurations are difficult to manage

- <b><i><span style="color:#d46644">Role Based Access Controls</span></i></b> (<b><i><span style="color:#d46644">RBAC</span></i></b>) make updating security changes much easier than <b><i><span style="color:#d46644">ABAC</span></i></b>

- With <b><i><span style="color:#d46644">RBAC</span></i></b>, instead of directly associating a <i><span style="color:#477bbe">user</span></i>/<i><span style="color:#477bbe">group</span></i> with a set of permissions, a role is defined and all <i><span style="color:#477bbe">users</span></i>/<i><span style="color:#477bbe">groups</span></i> that fall into that category are associated to the role
	- Ie. For developers, a role is created with a set of permissions for all developers

- With <b><i><span style="color:#d46644">RBAC</span></i></b>, whenever a change needs to be made to a <i><span style="color:#477bbe">users</span></i> access, rather than modifying individual policy definition files, simply modify the role and it will reflect on all <i><span style="color:#477bbe">users</span></i>/<i><span style="color:#477bbe">groups</span></i> in that category immediately

- <b><i><span style="color:#d46644">RBAC</span></i></b> provides a standard approach to managing access within the <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]]

- To outsource all of the <b><i><span style="color:#d46644">authorization</span></i></b> mechanisms, a <span style="color:#5c7e3e">webhook</span> is used

- <span style="color:#5c7e3e">Open Policy Agent</span> is a third-party tool that helps with admission control and <b><i><span style="color:#d46644">authorization</span></i></b>
	- <span style="color:#5c7e3e">Kubernetes</span> can make an API call to the <span style="color:#5c7e3e">Open Policy Agent</span> with information about the <i><span style="color:#477bbe">user</span></i> and their access requirements
	- <span style="color:#5c7e3e">Open Policy Agent</span> will then decide if the <i><span style="color:#477bbe">user</span></i> should be permitted

- The **Always Allow** mode allows all requests without performing any <b><i><span style="color:#d46644">authorization</span></i></b> checks

- The **Always Deny** mode denies all requests without performing any <b><i><span style="color:#d46644">authorization</span></i></b> checks

- The <b><i><span style="color:#d46644">authorization</span></i></b> modes are set using the <span style="color:red">--authorization-mode</span> option in the [[2 - Kube API server ✅|kube-apiserver]]
	- If this option is not set, it defaults to **Always Allow**

![[authorization-3.png]]

- If multiple <b><i><span style="color:#d46644">authorization</span></i></b> modes are necessary, a comma-separated list can be added to <span style="color:red">--authorization-mode</span> option in the [[2 - Kube API server ✅|kube-apiserver]]

![[authorization-4.png]]

- When multiple <b><i><span style="color:#d46644">authorization</span></i></b> modes are configured, requests are <b><i><span style="color:#d46644">authorized</span></i></b> using each mode in the order specified

- An example of multiple <b><i><span style="color:#d46644">authorization</span></i></b> modes is as follows:
	- Considering the [[0 - Core Concepts Intro ✅|node]], <b><i><span style="color:#d46644">RBAC</span></i></b>, <span style="color:#5c7e3e">Webhook</span> order:
		- When a <i><span style="color:#477bbe">user</span></i> sends a request it is first handled by the [[0 - Core Concepts Intro ✅|node]] <b><i><span style="color:#d46644">authorizer</span></i></b>
			- As the [[0 - Core Concepts Intro ✅|node]] <b><i><span style="color:#d46644">authorizer</span></i></b> only handles [[0 - Core Concepts Intro ✅|node]] requests (not <i><span style="color:#477bbe">user</span></i> requests), it denies the request
		- Whenever a mode denies a request, it is forwarded to the next one in the chain
		- The <b><i><span style="color:#d46644">RBAC</span></i></b> mode performs its checks and grants the <i><span style="color:#477bbe">user</span></i> permission
			- <b><i><span style="color:#d46644">Authorization</span></i></b> is then complete and the <i><span style="color:#477bbe">user</span></i> is given access
		- As soon as a mode approves a request, no more checks are done and the [[0 - Core Concepts Intro ✅|node]]/<i><span style="color:#477bbe">user</span></i>/<i><span style="color:#477bbe">group</span></i> is granted permission
			- Similar to when an if-statement finds its true conditional