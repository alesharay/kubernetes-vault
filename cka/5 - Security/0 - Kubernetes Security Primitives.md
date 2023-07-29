- All <b><i><span style="color:#d46644">access</span></i></b> to the hosts ([[0 - Core Concepts Intro|nodes]]) that form the cluster need to meet the following requirements:
	- must be secured
	- root <b><i><span style="color:#d46644">access</span></i></b> disabled
	- password based <b><i><span style="color:#d46644">authentication</span></i></b> disabled
	- only ssh key based <b><i><span style="color:#d46644">authentication</span></i></b> available

- If any of the security is compromised, everything is compromised

- The [[0 - Core Concepts Intro|kube-apiserver]] is the first line of defense

- With the [[0 - Core Concepts Intro|kube-apiserver]], two types of decisions need to be made:
	- Who can <b><i><span style="color:#d46644">access</span></i></b> the cluster 
	- What can the users with [[0 - Core Concepts Intro|cluster]] <b><i><span style="color:#d46644">access</span></i></b> do

- Who can <b><i><span style="color:#d46644">access</span></i></b> the [[0 - Core Concepts Intro|kube-apiserver]] is defined by <b><i><span style="color:#d46644">authentication</span></i></b> mechanisms

- There are different ways to <b><i><span style="color:#d46644">authenticate</span></i></b> to the [[0 - Core Concepts Intro|kube-apiserver]]
	- IDs and passwords stored in static files
	- Tokens stored in static files
	- Certificates
	- Integration with external <b><i><span style="color:#d46644">authentication</span></i></b> providers - LDAP
	- Service Accounts - Created for machines

- What can be done once a user gains <b><i><span style="color:#d46644">access</span></i></b> to the [[0 - Core Concepts Intro|cluster]] is defined by <b><i><span style="color:#d46644">authorization</span></i></b> mechanisms

- There are different ways to <b><i><span style="color:#d46644">authorize</span></i></b> as well
	- Role Based <b><i><span style="color:#d46644">Access</span></i></b> Control (<b><i><span style="color:#d46644">RBAC</span></i></b>) <b><i><span style="color:#d46644">Authorization</span></i></b>
	- Attribute Based <b><i><span style="color:#d46644">Access</span></i></b> Control (ABAC) <b><i><span style="color:#d46644">Authorization</span></i></b>
	- Node Authorizers
	- Webhooks

- <b><i><span style="color:#d46644">RBAC</span></i></b> is where users are associated with groups with specific permissions

- All communication with the [[0 - Core Concepts Intro|cluster]], and between various components, is secured using TLS Encryption

### Communication between apps in the cluster

- By default, all pods can <b><i><span style="color:#d46644">access</span></i></b> all other pods within the [[0 - Core Concepts Intro|cluster]]

- It's possible to restrict <b><i><span style="color:#d46644">access</span></i></b> between [[0 - Core Concepts Intro|clusters]] using network policies