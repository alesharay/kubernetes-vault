- Admins and other users of a cluster (developers, testers, etc…) are able to perform all types of operations in the cluster; thus authorization helps limit how much power a particular user has

- Ex. A developer shouldn't have the same access as an admin (ie. the ability to modify the kubeconfig file)

- There are different authorization mechanisms supported by Kubernetes

- Node Auth
- Attribute Based Auth (ABAC)
- Role Based Auth (RBAC)
- Webhook Auth
- * Always Allow
- * Always Deny

- The requests that kubelet makes to/from the kube-apiserver are made by a special handler known as the node authorizer

- This is because kubelet is a node

- Any request coming from a user with the name system:node and is part of the system:nodes group is authorized by the node authorizer and are granted those privileges

- Such as the privileges required by kubelet (this is access within the cluster)

- Attribute Based Authorization is where you associate a user or group of users with a set of permissions

- Ie.. The dev user can view, create, and delete pods
- (This is external access to the API)        

- To create Attribute Based Authorization, you use a set of policy files with the policies defined in JSON format

- This file is then passed into the API server

![[authorization-1.png]]

- With Attribute Based Authorization, a policy definition file is created for each user or group

- With Attribute Based Authorization, every time a change needs to be to the security, the policy definition file must be edited manually and the API server restarted

![[authorization-2.png]]

- Because the Attribute Based Access Control (ABAC) files have to be modified manually and the API server restarted when a change needs to be made, ABAC configurations are difficult to manage

- Role Based Access Controls (RBAC) make updating security changes much easier than ABAC

- With RBAC, instead of directly associating a user/group with a set of permissions, a role is defined and all users/groups that fall into that category are associated to the role

- Ie. For developers, a role is created with a set of permissions for all developers

- With RBAC, whenever a change needs to be made to a users access, rather than modifying individual policy definition files, simply modify the role and it will reflect on all users/groups in that category immediately

- RBAC provides a standard approach to managing access within the Kubernetes cluster

- To outsource all of the authorization mechanisms, a webhook is used

- Open Policy Agent is a third-party tool that helps with admission control and authorization

- Kubernetes can make an API call to the Open Policy Agent with information about the user and their access requirements
- Open Policy Agent will then decide if the user should be permitted

- The Always Allow mode allows all requests without performing any authorization checks

- The Always Deny mode denies all requests without performing any authorization checks

- The authorization modes are set using the <span style="color:red">--authorization-mode</span> option in the kube-apiserver

- If this option is not set, it defaults to Always Allow

![[authorization-3.png]]

- If multiple authorization modes are necessary, a comma-separated list can be added to <span style="color:red">--authorization-mode</span> option in the kube-apiserver

![[authorization-4.png]]

- When multiple authorization modes are configured, requests are authorized using each mode in the order specified

- An example of multiple authorization modes is as follows:

- Considering the node, RBAC, Webhook order:

- When a user sends a request it is first handled by the node authorizer

- As the node authorizer only handles node requests (not user requests), it denies the request

- Whenever a mode denies a request, it is forwarded to the next one in the chain
- The RBAC mode performs its checks and grants the user permission

- Authorization is then complete and the user is given access

- As soon as a mode approves a request, no more checks are done and the node/user/group is granted permission

- Similar to when an if-statement finds its true conditional