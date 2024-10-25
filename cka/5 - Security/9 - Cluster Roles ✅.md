#flashcards/kubernetes/cka/security

- <b><i><span style="color:#d46644">Roles</span></i></b> and <b><i><span style="color:#d46644">role bindings</span></i></b> are [[11 - Namespaces ✅|namespaced]], meaning they are created within [[11 - Namespaces ✅|namespaces]]

- If the [[11 - Namespaces ✅|namespace]] for <b><i><span style="color:#d46644">roles</span></i></b> and <b><i><span style="color:#d46644">role bindings</span></i></b> are not indicated, they will be created within the default [[11 - Namespaces ✅|namespace]] and can control resources in that [[11 - Namespaces ✅|namespace]] alone

- *Reminder: [[11 - Namespaces ✅|namespaces]] are used for grouping and isolating resources

- [[0 - Core Concepts Intro ✅|Nodes]] cannot be grouped inside of [[11 - Namespaces ✅|namespaces]] as they are <b><i><span style="color:#d46644">cluster-wide</span></i></b> or <b><i><span style="color:#d46644">cluster-scoped</span></i></b> resources
	- They cannot be associated with any particular [[11 - Namespaces ✅|namespace]]

- [[11 - Namespaces ✅|namespace-scoped]] resources included:

	* pods
	* replicaSets
	* jobs
	* deployments
	* services
	* secrets
	* roles
	* rolebindings
	* configmaps
	* persistent volume claims
	* etc…

- The <b><i><span style="color:#d46644">cluster-scoped</span></i></b> resources are those where you don't specify a [[11 - Namespaces ✅|namespace]]; such as:

	- nodes
	- persistent volumes
	- clusterroles
	- clusterrolebindings
	- certificate signing requests
	- namespaces
	- etc…

- To see a full list of [[11 - Namespaces ✅|namespaced]] and [[11 - Namespaces ✅|non-namespaced]] resources, run the <span style="color:red">kubectl api-resources</span> command with the [[11 - Namespaces ✅|namespaced]] option set

- To authorize <i><span style="color:#477bbe">users</span></i> to use <b><i><span style="color:#d46644">cluster-wide</span></i></b> resources like [[0 - Core Concepts Intro ✅|nodes]] or [[4 - Persistent Volumes ✅|persistent volumes]], use <b><i><span style="color:#d46644">clusterroles</span></i></b> and <b><i><span style="color:#d46644">clusterrolebindings</span></i></b>

- <b><i><span style="color:#d46644">Clusterroles</span></i></b> are just like <b><i><span style="color:#d46644">roles</span></i></b>, but at a cluster level
	- Ie. A <b><i><span style="color:#d46644">cluster admin role</span></i></b> can be created for the <b><i><span style="color:#d46644">cluster administrator</span></i></b>
	- Ie. A <b><i><span style="color:#d46644">storage admin role</span></i></b> can be created for the <b><i><span style="color:#d46644">storage administrator</span></i></b>

- To create a <b><i><span style="color:#d46644">clusterrole</span></i></b>, create a definition file with the kind "<span style="color:red">ClusterRole</span>" and specify the rules similar to those in a <b><i><span style="color:#d46644">role</span></i></b> definition file

![[cluster-1.png]]

- After creating a <b><i><span style="color:#d46644">clusterrole</span></i></b> object, the next step is to link a <i><span style="color:#477bbe">user</span></i>/<i><span style="color:#477bbe">group</span></i> to the <b><i><span style="color:#d46644">clusterrole</span></i></b>

- To link a <i><span style="color:#477bbe">user</span></i>/<i><span style="color:#477bbe">group</span></i> to a <b><i><span style="color:#d46644">clusterrole</span></i></b> object, create a <b><i><span style="color:#d46644">clusterrolebinding</span></i></b> object similar to a <b><i><span style="color:#d46644">rolebinding</span></i></b> object with the kind "<span style="color:red">ClusterRoleBinding</span>"

![[cluster-2.png]]

- It is not a hard rule that <b><i><span style="color:#d46644">clusterroles</span></i></b> and <b><i><span style="color:#d46644">clusterrolebindings</span></i></b> are used for [[0 - Core Concepts Intro ✅|clusters]] of resources
	- <b><i><span style="color:#d46644">Clusterroles</span></i></b> can be created for [[11 - Namespaces ✅|namespace]] resources as well
		- When a <b><i><span style="color:#d46644">clusterrole</span></i></b> is created for [[11 - Namespaces ✅|namespaced]] resources, the <i><span style="color:#477bbe">user</span></i> will have access to these resources across all [[11 - Namespaces ✅|namespaces]]

- <span style="color:#5c7e3e">Kubernetes</span> creates a number of <b><i><span style="color:#d46644">clusterroles</span></i></b> by default when the cluster is first setup

### Practice Problems

- How many ClusterRoles do you see defined in the cluster?

		kubectl get clusterroles --no-headers | wc -l

- How many ClusterRoleBindings exist on the cluster?

		kubectl get clusterrolebindings --no-headers | wc -l

- What namespace is the cluster-admin clusterrole part of?

	clusterroles are cluster-wide and not part of any namespace

- What user/groups are the cluster-admin role bound to?

		kubectl get clusterrolebinding cluster-admin

	(jot down users in subject section)

- What level of permission does the cluster-admin role grant?

		kubectl describe clusterrole clcuster-admin

	(jot down resource value)