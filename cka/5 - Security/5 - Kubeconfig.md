- A client can use the certificate file and key to query the Kubernetes REST API for a list of pods using curl

- This is then validated by the kube-apiserver to authenticate the user

![[kubeconfig-1.png]]

- For kubectl, the easiest way to query the Kubernetes REST API without having to pass in the certificate and key names each time, is by creating a kubeconfig file and specify its when using the <span style="color:red">kubectl get</span> command

![[kubeconfig-2.png]]

- By default, the kubectl tool looks for a file named config in the $HOME/.kube directory

- Therefore if the kubeconfig file is specified here, the <span style="color:red">--kubeconfig</span> option is not necessary for kubectl API calls

- The kubeconfig file is in a specific format of the following sections:

- clusters
- users
- contexts
- current-context
- preferences

- Clusters (on the kubeconfig files) are the various kubernetes clusters that you need access to

- Users (on the kubeconfig file) are the user accounts with which you have access to the specified clusters

- These users may have different privileges on different clusters

- Contexts (on the kubeconfig file) marry the users and clusters together by defining which user account will be used to access which cluster

- In the kubeconfig file, the server specification and CA cert  info would go into the cluster section

- In the kubeconfig file, the admin user keys and certificates, goes into the users section

- In the kubeconfig file, once the cluster and user sections are setup, you then setup contexts to specify which user account will access which cluster

- The current apiVersion for a kubeconfig file is v1 and the kind is config

- Each of the kubeconfig sections (clusters, contexts, users) are in array format so that you can specify multiple of each in the same file

- In the kubeconfig file, each cluster has the following properties:

- name (string/property): name of the cluster
- cluster (dictionary): details of the cluster

- *certificate-authority (string/property): the full path to the CA cert file
- *certificate-authority-data (string/property): the base64 encoded contents of the CA cert

- ( * ) = only one of these fields are needed

- server:  the DNS for the kube-apiserver

- In the kubeconfig file, the user section has the following properties:

- name (string/property): name of the user account
- User (dictionary: details of the user

- client-certificate (string/property): the signed TLS certificate for the user account (either filename/location or base64 encoded string)
- client-key (string/property): the private key for the user account (filename/location or base64 encoded string)

- In the kubeconfig file, the contexts section has the following properties:

- name (string/property): whatever name you want to give the context (may be helpful the indicate the user and cluster in the name)
- context (dictionary): details of the user and cluster connection

- cluster (string/property): cluster name for the context
- user (string/property): user name for the specified cluster
- namespace (string/property): specifies a particular namespace for that context so that when that context is used, you will automatically be in the correct namespace

- In the kubeconfig file, the current-context section specifies the default context to use when connecting to Kubernetes

- There are command line options available within the kubectl tool to view and modify the kubeconfig file

- To view the kubeconfig file being used, run the <span style="color:red">kubectl config view</span> command

- This command lists the clusters, contexts and users as well as the current-context

![[kubeconfig-3.png]]

- If you don't want to use the default kubeconfig file when running the <span style="color:red">kubectl config</span> command, you can pass in the <span style="color:red">--kubeconfig=</span> command with the location of the desired kubeconfig file

![[kubeconfig-4.png]]

- To update the current-context, run the <span style="color:red">kubectl config use-context</span> command with the name of the desired context

- These changes will be reflected in the output of the command

![[kubeconfig-5.png]]

- Other changes can be made to the kubeconfig file using other variations of the <span style="color:red">kubectl config</span> command

- Use <span style="color:red">-h</span> to get a list of available options

![[kubeconfig-6.png]]

### Practice Questions

- Where is the default kubeconfig file located in the current environment?

	(find the current home directory by outputting the value of the $HOME variable)

		echo $HOME
		ls $HOME/.kube/config

- How many clusters are defined in the default kubeconfig file?

		kubectl config view

	(count the number of clusters)

- How many users are defined in the default kubeconfig file?

		kubectl config view

	(count the number of users)

- How many contexts are defined in the default kubeconfig file?

		kubectl config view

	(count the number of contexts)

- What is the user configured in the current context?

		kubectl config view

	(check the name of the current context, then look at that context to determine the user)

- What is the name of the cluster configured in the default kubeconfig file

		kubectl config view

	(jot down the name of the only cluster)

- A new kubeconfig file named "my-kube-config" is created. It is placed in the /root directory. How many clusters are defined in that kubeconfig file?

		cat /root/my-kube-config

	(count the number of clusters)

- How many contexts are configured in the "my-kube-config" file?

		cat /root/my-kube-config

	(count the number of contexts)

- What user is configured in the Dev context?

		cat /root/my-kube-config

	(jot down the name of the user specified in the Dev context)

- Set the current context for the "my-kube-config" file to use the "dev-user" to access "test-cluster-1"

		kubectl config use-context research --kubeconfig=/root/my-kube-config

- With the current context set to research, we are trying to access the cluster. However, something seems to be wrong. Identify and fix the issue.

		kubectl get pods

	(identify and fix the issue)