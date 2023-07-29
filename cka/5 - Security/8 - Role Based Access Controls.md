- A role is created by creating a role object

- The apiVersion for a role definition file is "rbac.authorization.k8s.io/v1"

- The kind for a role definition file is "Role"

- The metadata property has a label called name with the name of who/what the role is for

- Ie. Developer, if the role is for developers

![[role-based-1.png]]

- On a role definition file, instead of the spec section, a rules section is defined

- On a role definition file, each rule has three sections:

- apiGroups
- resources
- verbs

- For a core group, the apiGroups section of the role definition file can be left blank. But for any other group, the group name must be specified

- The resources property of the rules section on a role definition is for what you want to provide access to

- Ie. For developers, you provide access to pods

- The verbs property of the rules section on a role definition is for the actions the user can take

- Ie. For developers, the can perform the "list, get, create, update, delete" actions

- Each resource gets its own set of rules

![[role-based-2.png]]

- After a role is created, the next step is to link a user to that role

- To link a user to a role, an object called a role binding object is created

- The role binding object links a user object to a role object

- The apiVersion for a role binding object is the same as that for a role object; "rbac.authorization.k8s.io/v1"

- The kind for a role binding object is RoleBinding

- For role binding objects, instead of the spec section, there are two sections:

- subjects
- roleRef

- In a role binding object, the subjects section is where user details are specified

- In a role binding object, the roleRef section is where the role details are specified

![[role-based-3.png]]

- The roles and role bindings fall under the scope of namespaces

- To limit the access of a user to a specific namespace; specify the namespace in the metadata of the role and role binding definition files

- To view the created roles, use the <span style="color:red">kubectl get roles</span> command

![[role-based-4.png]]

- To view details about a role, use the <span style="color:red">kubectl describe role</span> command

- Here you see details about resources and permissions for each resources

![[role-based-5.png]]

- To list the role bindings, use the <span style="color:red">kubectl get rolebindings</span> command

![[role-based-6.png]]

- To view details about role bindings, use the <span style="color:red">kubectl describe rolebindings</span> command

- Here you can see details about an existing role binding

![[role-based-7.png]]

- As a user, if you want to see if you have access to a particular resource in the cluster, you can use the <span style="color:red">kubectl auth can-i</span>command with the action and resource

- You will get a boolean response on whether you can or can't perform that action on the resource

![[role-based-8.png]]

- Admins can impersonate other users to check their access by passing in the <span style="color:red">--as</span> option with the user name

![[role-based-9.png]]

- When checking user access, the namespace can be specified by using the <span style="color:red">--namespace</span> option

!![[role-based-10.png]]

- In addition to providing rules for a specific resource (like pods), you can also go one level in and provide rules for a specific resources within that group (like green pod or blue pod) by specifying the resourceNames property

![[role-based-11.png]]

### Practice Problems

- Inspect the environment and identify the authorization modes configured on the cluster

		kubectl describe pod <kube-apiserver pod name> -n kube-system

	(jot down the name of the modes in the "--authorization-mode" option)

- How many roles exist in the default namespace?

		kubectl get roles

	(jot down the number of roles)

- What are the resources for one of the existing roles?

		kubectl describe role <role name>

	(jot down resources for the role)

- What actions can the role perform on the chosen resource?

		kubectl describe role <role name>

	(jot down the allowed actions)

- Which account is the chosen role assigned to?

		kubectl describe rolebinding <rolebinding name that ties to chosen role name>

	(jot down the account name for the rolebinding)

- Choose a user and a resource. Check if the user can list values on the chosen resource

		kubectl auth can-i list <resource name> --as <chosen user>

- Create the necessary roles and role bindings required for the chosen user to create, list, and delete the resource

See docs or notes on creating roles and rolebindings