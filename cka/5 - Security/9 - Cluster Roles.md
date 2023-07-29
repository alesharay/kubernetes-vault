- Roles and role bindings are namespaced, meaning they are created within namespaces

- If the namespace for roles and role bindings are not indicated, they will be created within the default namespace and can control resources in that namespace alone

- *Reminder: namespaces are used for grouping and isolating resources

- Nodes cannot be grouped inside of namespaces as they are cluster-wide or cluster-scoped resources

- They cannot be associated with any particular namespace

- Namespace-scoped resources included:

Pods, replicasets, jobs, deployments, services, secrets, roles, rolebindings, configmaps, persistent volume claims, etc…

- The cluster-scoped resources are those where you don't specify a namespace; such as:

Nodes, persistent volumes, clusterroles, clusterrolebindings, certificate signing requests, namespaces, etc…

- To see a full list of namespaced and non-namespaced resources, run the `kubectl api-resource` command with the namespaced option set

- To authorize users to use cluster-wide resources like nodes or persistent volumes, use clusterroles and clusterrolebindings

- Clusterroles are just like roles, only at a cluster level

- Ie. A cluster admin role can be created for the cluster administrator
- Ie. A storage admin role can be created for the storage administrator

- To create a clusterrole, create a clusterrole definition file with the kind "ClusterRole" and specify the rules similar to those in a role definition file

![[cluster-1.png]]

- After creating a clusterrole object, the next step is to link a user/group to the clusterrole

- To link a user/group to a clusterrole object, create a clusterrolebinding object similar to a rolebinding object with the kind "ClusterRoleBinding"

![[cluster-2.png]]

- It is not a hard rule that clusterroles and clusterrolebindings are used for clusters of resources

- Clusterroles can be created for namespace resources as well

- When a clusterrole is created for namespaced resources, the user will have access to these resources across all namespaces

- Kubernetes creates a number of clusterroles by default when the cluster is first setup

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