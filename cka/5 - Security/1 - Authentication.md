- Different <i><span style="color:#477bbe">groups</span></i> must access the [[0 - Core Concepts Intro|node]] to perform different tasks
	- Admins for administrative tasks
	- Developers for testing and deploying applications
	- End <i><span style="color:#477bbe">users</span></i> for accessing the deployed applications
	- Third party applications for integration purposes

- One way to <b><i><span style="color:#d46644">secure</span></i></b> the [[0 - Core Concepts Intro|cluster]] is to <b><i><span style="color:#d46644">secure</span></i></b> the communication between internal components and securing management access to the [[0 - Core Concepts Intro|cluster]] through <b><i><span style="color:#d46644">authentication</span></i></b> and [[7 - Authorization|authorization]] mechanisms

- The <b><i><span style="color:#d46644">security</span></i></b> of <i><span style="color:#477bbe">end-users</span></i> who access the <u>applications</u> deployed on the [[0 - Core Concepts Intro|cluster]] is managed by the applications themselves internally

- <span style="color:#5c7e3e">Kubernetes</span> does not manage <i><span style="color:#477bbe">user</span></i> accounts natively, it relies on external sources like a file with <i><span style="color:#477bbe">user</span></i> details, certificates or a third-party <b><i><span style="color:#d46644">security</span></i></b> [[10 - Services|service]] like <b><i><span style="color:#d46644">LDAP</span></i></b> to manage these <i><span style="color:#477bbe">users</span></i>.

- In case of [[10 - Service Accounts|Service Accounts]], <span style="color:#5c7e3e">Kubernetes</span> can manage them

- You can create and manage [[10 - Service Accounts|Service Accounts]] using the <span style="color:#5c7e3e">Kubernetes</span> API

        kubernetes create serviceaccount SERVICE_ACCOUNT_NAME

![[auth-1.png]]

![[auth-2.png]]

- All <i><span style="color:#477bbe">user</span></i> access is managed by the [[2 - Kube API server|API Server]]

- Whether you are accessing the [[0 - Core Concepts Intro|cluster]] through the kubectl tool or the API directly, all of the request go through the [[2 - Kube API server|kube-apiserver]]

- The [[2 - Kube API server|kube-apiserver]] <b><i><span style="color:#d46644">authenticates</span></i></b> the requests before processing it

![[auth-3.png]]

### How does the [[2 - Kube API server|kube-apiserver]] <b><i><span style="color:#d46644">authenticate</span></i></b>?

- For [[2 - Kube API server|kube-apiserver]] <i><span style="color:#477bbe">user</span></i> <b><i><span style="color:#d46644">authentication</span></i></b>
	- you can have a list of usernames and passwords in a <u><i>static password</i></u> file
	- you can have usernames and tokens in a <u><i>static token</i></u> file
	- you can <b><i><span style="color:#d46644">authenticate</span></i></b> using certificates
	- you can connect to a third party <b><i><span style="color:#d46644">authentication</span></i></b> protocol like <b><i><span style="color:#d46644">LDAP</span></i></b> or Kerberos

![[auth-4.png]]

- The simplest form of <b><i><span style="color:#d46644">authentication</span></i></b> is using <u><i>static password</i></u> and token files

- You can create a list of <i><span style="color:#477bbe">users</span></i> and their passwords in a csv file and use that as the source for <i><span style="color:#477bbe">user</span></i> information
	- The file has three columns
		- Username
		- Password
		- <i><span style="color:#477bbe">user</span></i> ID

- After the <u><i>static</i></u> <b><i><span style="color:#d46644">authentication</span></i></b> file is created, we then pass the file name as an option to the [[2 - Kube API server|kube-apiserver]] service file

![[auth-5.png]]

![[auth-6.png]]

- After the <u><i>static</i></u> <b><i><span style="color:#d46644">authentication</span></i></b> file has been passed to the [[2 - Kube API server|kube-apiserver]] service, you must restart the [[2 - Kube API server|kube-apiserver]] for these options to take effect

- If you setup your [[0 - Core Concepts Intro|cluster]] using the <span style="color:#5c7e3e">Kubeadm</span> tool, you must modify the [[2 - Kube API server|kube-apiserver]] pod definition file
	- The <span style="color:#5c7e3e">Kubeadm</span> tool will automatically restart the [[2 - Kube API server|kube-apiserver]] once you update the file

- To <b><i><span style="color:#d46644">authenticate</span></i></b> using the basic credentials while accessing the [[2 - Kube API server|API Server]] specify the <i><span style="color:#477bbe">user</span></i> and password in a curl command as follows

![[auth-7.png]]

- In the <u><i>static</i></u> <i><span style="color:#477bbe">user</span></i> <b><i><span style="color:#d46644">authentication</span></i></b> csv file, there can be an optional 4th column with the group details to assign <i><span style="color:#477bbe">users</span></i> to specific <i><span style="color:#477bbe">groups</span></i> similarly instead of a <u><i>static password</i></u> file

![[auth-8.png]]

- You can have a <u><i>static token</i></u> file where instead of a password, you specify a token

![[auth-9.png]]

- Pass the token file as an option to the [[2 - Kube API server|kube-apiserver]]

![[auth-10.png]]

- While <b><i><span style="color:#d46644">authenticating</span></i></b>, specify the token as an [[7 - Authorization|authorization]] bearer token to your request

![[auth-11.png]]

- Storing <b><i><span style="color:#d46644">authentication</span></i></b> information in clear text in a file and passing it in is not the recommended approach for <b><i><span style="color:#d46644">authentication</span></i></b> as it is not <b><i><span style="color:#d46644">secure</span></i></b>
	- Consider volume mount while providing the <b><i><span style="color:#d46644">authentication</span></i></b> file in a <span style="color:#5c7e3e">Kubeadm</span> setup
	- Setup <b><i><span style="color:#d46644">RBAC</span></i></b> for the new <i><span style="color:#477bbe">users</span></i>