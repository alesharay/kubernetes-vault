- A <b><i><span style="color:#d46644">role</span></i></b> is created by creating a <b><i><span style="color:#d46644">role</span></i></b> object

- The <span style="color:#5c7e3e">apiVersion</span> for a <b><i><span style="color:#d46644">role</span></i></b> definition file is "rbac.authorization.k8s.io/v1"

- The <span style="color:#5c7e3e">kind</span> for a <b><i><span style="color:#d46644">role</span></i></b> definition file is "<span style="color:red">Role</span>"

- The <span style="color:#5c7e3e">metadata</span> property has a <span style="color:#5c7e3e">label</span> called name with the name of who/what the <b><i><span style="color:#d46644">role</span></i></b> is for
	- Ie. Developer, if the <b><i><span style="color:#d46644">role</span></i></b> is for developers

![[role-based-1.png]]

- On a <b><i><span style="color:#d46644">role</span></i></b> definition file, instead of the <span style="color:#5c7e3e">spec</span> section, a <span style="color:#5c7e3e">rules</span> section is defined

- On a <b><i><span style="color:#d46644">role</span></i></b> definition file, each rule has three sections:
	- apiGroups
	- resources
	- verbs

- For a [[6 - API Groups ✅|core group]], the [[6 - API Groups ✅|apiGroups]] section of the <b><i><span style="color:#d46644">role</span></i></b> definition file can be left blank. But for any other <i><span style="color:#477bbe">group</span></i>, the <i><span style="color:#477bbe">group</span></i> name must be specified

- The <span style="color:#5c7e3e">resources</span> property of the <span style="color:#5c7e3e">rules</span> section on a <b><i><span style="color:#d46644">role</span></i></b> definition is for what you want to provide access to
	- Ie. For developers, you provide access to [[7 - Pods ✅|pods]]

- The verbs property of the <span style="color:#5c7e3e">rules</span> section on a <b><i><span style="color:#d46644">role</span></i></b> definition is for the actions the <i><span style="color:#477bbe">user</span></i> can take
	- Ie. For developers, the can perform the "list, get, create, update, delete" actions

- Each <b><i><span style="color:#d46644">resource</span></i></b> gets its own set of <b><i><span style="color:#d46644">rules</span></i></b>

![[role-based-2.png]]

- After a <b><i><span style="color:#d46644">role</span></i></b> is created, the next step is to link a <i><span style="color:#477bbe">user</span></i> to that <b><i><span style="color:#d46644">role</span></i></b>

- To link a <i><span style="color:#477bbe">user</span></i> to a <b><i><span style="color:#d46644">role</span></i></b>, an object called a <b><i><span style="color:#d46644">role binding</span></i></b> object is created

- The <b><i><span style="color:#d46644">role binding</span></i></b> object links a <i><span style="color:#477bbe">user</span></i> object to a <b><i><span style="color:#d46644">role</span></i></b> object

- The <span style="color:#5c7e3e">apiVersion</span> for a <b><i><span style="color:#d46644">role binding</span></i></b> object is the same as that for a <b><i><span style="color:#d46644">role</span></i></b> object; "rbac.authorization.k8s.io/v1"

- The <span style="color:#5c7e3e">kind</span> for a <b><i><span style="color:#d46644">role binding</span></i></b> object is <span style="color:red">RoleBinding</span>

- For <b><i><span style="color:#d46644">role binding</span></i></b> objects, instead of the <span style="color:#5c7e3e">spec</span> section, there are two sections:
	- subjects
	- roleRef

- In a <b><i><span style="color:#d46644">role binding</span></i></b> object, the <span style="color:#5c7e3e">subjects</span> section is where <i><span style="color:#477bbe">user</span></i> details are specified

- In a <b><i><span style="color:#d46644">role binding</span></i></b> object, the <span style="color:#5c7e3e">roleRef</span> section is where the <b><i><span style="color:#d46644">role</span></i></b> details are specified

![[role-based-3.png]]

- The <b><i><span style="color:#d46644">roles</span></i></b> and <b><i><span style="color:#d46644">role bindings</span></i></b> fall under the scope of [[11 - Namespaces ✅|namespaces]]

- To limit the access of a <i><span style="color:#477bbe">user</span></i> to a specific [[11 - Namespaces ✅|namespace]]; specify the [[11 - Namespaces ✅|namespace]] in the <span style="color:#5c7e3e">metadata</span> of the <b><i><span style="color:#d46644">role</span></i></b> and <b><i><span style="color:#d46644">role binding</span></i></b> definition files

- To view the created <b><i><span style="color:#d46644">roles</span></i></b>, use the <span style="color:red">kubectl get roles</span> command

![[role-based-4.png]]

- To view details about a <b><i><span style="color:#d46644">role</span></i></b>, use the <span style="color:red">kubectl describe role</span> command
	- Here you see details about <b><i><span style="color:#d46644">resources</span></i></b> and permissions for each <b><i><span style="color:#d46644">resource</span></i></b>

![[role-based-5.png]]

- To list the <b><i><span style="color:#d46644">role bindings</span></i></b>, use the <span style="color:red">kubectl get rolebindings</span> command

![[role-based-6.png]]

- To view details about <b><i><span style="color:#d46644">role bindings</span></i></b>, use the <span style="color:red">kubectl describe rolebindings</span> command
	- Here you can see details about an existing <b><i><span style="color:#d46644">role binding</span></i></b>

![[role-based-7.png]]

- As a <i><span style="color:#477bbe">user</span></i>, if you want to see if you have access to a particular <b><i><span style="color:#d46644">resource</span></i></b> in the [[0 - Core Concepts Intro ✅|cluster]], you can use the <span style="color:red">kubectl auth can-i</span> command with the action and <b><i><span style="color:#d46644">resource</span></i></b>
	- You will get a boolean response on whether you can or can't perform that action on the <b><i><span style="color:#d46644">resource</span></i></b>

![[role-based-8.png]]

- <i><span style="color:#477bbe">Admins</span></i> can impersonate other <i><span style="color:#477bbe">users</span></i> to check their access by passing in the <span style="color:red">--as</span> option with the username

![[role-based-9.png]]

- When checking <i><span style="color:#477bbe">user</span></i> access, the [[11 - Namespaces ✅|namespace]] can be specified by using the <span style="color:red">--namespace</span> option

!![[role-based-10.png]]

- In addition to providing <b><i><span style="color:#d46644">rules</span></i></b> for a specific <b><i><span style="color:#d46644">resource</span></i></b> (like [[7 - Pods ✅|pods]]), you can also go one level in and provide <b><i><span style="color:#d46644">rules</span></i></b> for a specific <b><i><span style="color:#d46644">resources</span></i></b> within that <i><span style="color:#477bbe">group</span></i> (like green [[7 - Pods ✅|pod]] or blue [[7 - Pods ✅|pod]]) by specifying the <span style="color:#5c7e3e">resourceNames</span> property

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