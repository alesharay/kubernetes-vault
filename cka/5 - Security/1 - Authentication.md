- Different groups must access the node to perform different tasks

- Admins for administrative tasks
- Developers for testing and deploying applications
- End users for accessing the deployed applications
- Third party applications for integration purposes

- One way to secure the cluster is to secure the communication between internal components and securing management access to the cluster through authentication and authorization mechanisms

- The security of end-users who access the applications deployed on the cluster is managed by the applications themselves internally

- Kubernetes does not manage user accounts natively, it relies on external sources like a file with user details, certificates or a third-party security service like LDAP to manage these users.

- In case of Service Accounts, Kubernetes can manage them

- You can create and manage Service Accounts using the Kubernetes API

        kubernetes create serviceaccount SERVICE_ACCOUNT_NAME

![[auth-1.png]]

![[auth-2.png]]

- All user access is managed by the API server

- Whether you are accessing the cluster through the kubectl tool or the API directly, all of the request go through the kube-apiserver

- The kube-apiserver authenticates the requests before processing it

![[auth-3.png]]

How does the kube-apiserver authenticate?

- For kube-apiserver user authentication

- you can have a list of usernames and passwords in a static password file
- you can have usernames and tokens in a static token file
- you can authenticate using certificates
- you can connect to a third party authentication protocol like LDAP or Kerberos

![[auth-4.png]]

- The simplest form of authentication is using static password and token files

- You can create a list of users and their passwords in a csv file and use that as the source for user information

- The file has three columns

- Username
- Password
- User ID

- After the static authentication file is created, we then pass the file name as an option to the kube-apiserver service file

![[auth-5.png]]

![[auth-6.png]]

- After the static authentication file has been passed to the kube-apiserver service, you must restart the kube-apiserver for these options to take effect

- If you setup your cluster using the kubeadm tool, you must modify the kube-apiserver pod definition file

- The kubeadm tool will automatically restart the kube-apiserver once you update the file

- To authenticate using the basic credentials while accessing the API server specify the user and password in a curl command as follows

![[auth-7.png]]

- In the static user authentication csv file, there can be an optional 4th column with the group details to assign users to specific groups similarly instead of a static password file

![[auth-8.png]]

- You can have a static token file where instead of a password, you specify a token

![[auth-9.png]]

- Pass the token file as an option to the kube-apiserver

![[auth-10.png]]

- While authenticating, specify the token as an authorization bearer token to your request

![[auth-11.png]]

- Storing authentication information in clear text in a file and passing it in is not the recommended approach for authentication as it is not secure

- Consider volume mount while providing the authentication file in a kubeadm setup
- Setup RBAC for the new users