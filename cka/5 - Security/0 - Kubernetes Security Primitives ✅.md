- All <b><i><span style="color:#d46644">access</span></i></b> to the hosts ([[0 - Core Concepts Intro ✅|nodes]]) that form the [[0 - Core Concepts Intro ✅|cluster]] need to meet the following requirements:
	- must be <b><i><span style="color:#d46644">secured</span></i></b>
	- root <b><i><span style="color:#d46644">access</span></i></b> disabled
	- password based  [[1 - Authentication|authentication]]  disabled
	- only ssh key based [[1 - Authentication|authentication]] available

* If <b><i><span style="color:#d46644">security</span></i></b> is compromised, everything is compromised

- The [[0 - Core Concepts Intro ✅|kube-apiserver]] is the first line of defense

- With the [[0 - Core Concepts Intro ✅|kube-apiserver]], two types of decisions need to be made:
	- Who can <b><i><span style="color:#d46644">access</span></i></b> the [[0 - Core Concepts Intro ✅|cluster]] 
	- What can the <i><span style="color:#477bbe">users</span></i> with [[0 - Core Concepts Intro ✅|cluster]] <b><i><span style="color:#d46644">access</span></i></b> do

- Who can <b><i><span style="color:#d46644">access</span></i></b> the [[0 - Core Concepts Intro ✅|kube-apiserver]] is defined by [[1 - Authentication|authentication]] mechanisms

- There are different ways to <b><i><span style="color:#d46644">authenticate</span></i></b> to the [[0 - Core Concepts Intro ✅|kube-apiserver]]
	- IDs and passwords stored in static files
	- Tokens stored in static files
	- Certificates
	- Integration with external [[1 - Authentication|authentication]] providers - LDAP
	- Service Accounts - Created for machines

- What can be done, once a <i><span style="color:#477bbe">user</span></i> gains <b><i><span style="color:#d46644">access</span></i></b> to the [[0 - Core Concepts Intro ✅|cluster]], is defined by [[7 - Authorization|authorization]] mechanisms

- There are different ways to <b><i><span style="color:#d46644">authorize</span></i></b> as well
	- Role Based <b><i><span style="color:#d46644">Access</span></i></b> Control (<b><i><span style="color:#d46644">RBAC</span></i></b>) [[7 - Authorization|authorization]]
	- Attribute Based <b><i><span style="color:#d46644">Access</span></i></b> Control (ABAC) [[7 - Authorization|authorization]]
	- Node Authorizers
	- Webhooks

- <b><i><span style="color:#d46644">RBAC</span></i></b> is where <i><span style="color:#477bbe">users</span></i> are associated with groups with specific permissions

- All communication with the [[0 - Core Concepts Intro ✅|cluster]], and between various components, is <b><i><span style="color:#d46644">secured</span></i></b> using <b><i><span style="color:#d46644">TLS Encryption</span></i></b>

### Communication between apps in the cluster

- By default, all [[7 - Pods ✅|pods]] can <b><i><span style="color:#d46644">access</span></i></b> all other [[7 - Pods ✅|pods]] within the [[0 - Core Concepts Intro ✅|cluster]]

- It's possible to restrict <b><i><span style="color:#d46644">access</span></i></b> between [[0 - Core Concepts Intro ✅|clusters]] using network policies