#flashcards/kubernetes/cka/security

- A <i><span style="color:#477bbe">client</span></i> can use the [[3.1 - Certification Creation|certificate]] file and [[3.1 - Certification Creation|key]] to query the <span style="color:#5c7e3e">Kubernetes</span> REST API for a list of [[7 - Pods ✅|pods]] using <span style="color:#5c7e3e">curl</span>
	- This is then validated by the [[2 - Kube API server ✅|kube-apiserver]] to [[1 - Authentication ✅|authenticate]] the <i><span style="color:#477bbe">user</span></i>

![[kubeconfig-1.png]]

- For [[13 - Kubectl Apply ✅|kubectl]], the easiest way to query the <span style="color:#5c7e3e">Kubernetes</span> REST API without having to pass in the [[3.1 - Certification Creation|certificate]] and [[3.1 - Certification Creation|key]] names each time, is by creating a <b><i><span style="color:#d46644">kubeconfig</span></i></b> file and specify it when using the <span style="color:red">kubectl get</span> command

![[kubeconfig-2.png]]

- By default, the [[13 - Kubectl Apply ✅|kubectl]] tool looks for a file named config in the <span style="color:red">$HOME/.kube</span> directory
	- Therefore if the <b><i><span style="color:#d46644">kubeconfig</span></i></b> file is specified here, the <span style="color:red">--kubeconfig</span> option is not necessary for [[13 - Kubectl Apply ✅|kubectl]] API calls

- The <b><i><span style="color:#d46644">kubeconfig</span></i></b> file is in a specific format of the following sections:
	- clusters
	- users
	- contexts
	- current-context
	- preferences

- [[0 - Core Concepts Intro ✅|Clusters]] (on the <b><i><span style="color:#d46644">kubeconfig</span></i></b> files) are the various <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|clusters]] that you need access to

- <i><span style="color:#477bbe">Users</span></i> (on the <b><i><span style="color:#d46644">kubeconfig</span></i></b> file) are the <i><span style="color:#477bbe">user accounts</span></i> with which you have access to the specified [[0 - Core Concepts Intro ✅|clusters]]
	- These <i><span style="color:#477bbe">users</span></i> may have different privileges on different [[0 - Core Concepts Intro ✅|clusters]]

- <b><i><span style="color:#d46644">Contexts</span></i></b> (on the <b><i><span style="color:#d46644">kubeconfig</span></i></b> file) marry the <i><span style="color:#477bbe">users</span></i> and [[0 - Core Concepts Intro ✅|clusters]] together by defining which <i><span style="color:#477bbe">user account</span></i> will be used to access which [[0 - Core Concepts Intro ✅|cluster]]

- In the <b><i><span style="color:#d46644">kubeconfig</span></i></b> file, the <i><span style="color:#477bbe">server</span></i> specification and [[3.1 - Certification Creation|CA cert]] info would go into the [[0 - Core Concepts Intro ✅|cluster]] section

- In the <b><i><span style="color:#d46644">kubeconfig</span></i></b> file, the <i><span style="color:#477bbe">admin</span></i> [[3.1 - Certification Creation|user keys]] and [[3.1 - Certification Creation|certificates]], goes into the <i><span style="color:#477bbe">users</span></i> section

- In the <b><i><span style="color:#d46644">kubeconfig</span></i></b> file, once the [[0 - Core Concepts Intro ✅|cluster]] and <i><span style="color:#477bbe">user</span></i> sections are setup, you then setup <b><i><span style="color:#d46644">contexts</span></i></b> to specify which <i><span style="color:#477bbe">user account</span></i> will access which [[0 - Core Concepts Intro ✅|cluster]]

- The current <span style="color:#5c7e3e">apiVersion</span> for a <b><i><span style="color:#d46644">kubeconfig</span></i></b> file is v1 and the <span style="color:#5c7e3e">kind</span> is <b><i><span style="color:#d46644">Config</span></i></b>

- Each of the <b><i><span style="color:#d46644">kubeconfig</span></i></b> sections ([[0 - Core Concepts Intro ✅|Cluster]], <b><i><span style="color:#d46644">contexts</span></i></b>, <i><span style="color:#477bbe">users</span></i>) are in array format so that you can specify multiple of each in the same file

- In the <b><i><span style="color:#d46644">kubeconfig</span></i></b> file, each [[0 - Core Concepts Intro ✅|cluster]] has the following properties:
	- name (string/property): name of the [[0 - Core Concepts Intro ✅|cluster]]
	- [[0 - Core Concepts Intro ✅|cluster]] (dictionary): details of the [[0 - Core Concepts Intro ✅|cluster]]
		- *[[3.1 - Certification Creation|certificate-authority]] (string/property): the full path to the [[3.1 - Certification Creation|CA cert]] file
		- *[[3.1 - Certification Creation|certificate-authority-data]] (string/property): the <span style="color:#5c7e3e">base64</span> encoded contents of the [[3.1 - Certification Creation|CA cert]]
			- ( * ) = only one of these fields are needed
		- <i><span style="color:#477bbe">server</span></i>:  the DNS for the [[2 - Kube API server ✅|kube-apiserver]]

- In the <b><i><span style="color:#d46644">kubeconfig</span></i></b> file, each <i><span style="color:#477bbe">user</span></i> has the following properties:
	- name (string/property): name of the <i><span style="color:#477bbe">user account</span></i>
	- user (dictionary): details of the user
		- [[3.1 - Certification Creation|client-certificate]] (string/property): the signed [[3.1 - Certification Creation|TLS certificate]] for the <i><span style="color:#477bbe">user account</span></i> (either filename/location or <span style="color:#5c7e3e">base64</span> encoded string)
		- [[3.1 - Certification Creation|client-key]] (string/property): the [[3.1 - Certification Creation|private key]] for the <i><span style="color:#477bbe">user account</span></i> (filename/location or <span style="color:#5c7e3e">base64</span> encoded string)

- In the <b><i><span style="color:#d46644">kubeconfig</span></i></b> file, the <b><i><span style="color:#d46644">contexts</span></i></b> section has the following properties:
	- name (string/property): whatever name you want to give the <b><i><span style="color:#d46644">context</span></i></b> (may be helpful to indicate the <i><span style="color:#477bbe">user</span></i> and [[0 - Core Concepts Intro ✅|cluster]] in the name)
	- <b><i><span style="color:#d46644">context</span></i></b> (dictionary): details of the <i><span style="color:#477bbe">user</span></i> and [[0 - Core Concepts Intro ✅|cluster]] connection
		- [[0 - Core Concepts Intro ✅|cluster]] (string/property): [[0 - Core Concepts Intro ✅|cluster]] name for the <b><i><span style="color:#d46644">context</span></i></b>
		- <i><span style="color:#477bbe">user</span></i> (string/property): <i><span style="color:#477bbe">username</span></i> for access to the specified [[0 - Core Concepts Intro ✅|cluster]]
		- [[11 - Namespaces ✅|namespace]] (string/property): specifies a particular [[11 - Namespaces ✅|namespace]] for that <b><i><span style="color:#d46644">context</span></i></b> so that when that <b><i><span style="color:#d46644">context</span></i></b> is used, you will automatically be in the correct [[11 - Namespaces ✅|namespace]]

- In the <b><i><span style="color:#d46644">kubeconfig</span></i></b> file, the <b><i><span style="color:#d46644">current-context</span></i></b> section specifies the default <b><i><span style="color:#d46644">context</span></i></b> to use when connecting to <span style="color:#5c7e3e">Kubernetes</span>

- There are command line options available within the [[13 - Kubectl Apply ✅|kubectl]] tool to view and modify the <b><i><span style="color:#d46644">kubeconfig</span></i></b> file

- To view the <b><i><span style="color:#d46644">kubeconfig</span></i></b> file being used, run the <span style="color:red">kubectl config view</span> command
	- This command lists the [[0 - Core Concepts Intro ✅|clusters]], <b><i><span style="color:#d46644">contexts</span></i></b> and <i><span style="color:#477bbe">users</span></i> as well as the <b><i><span style="color:#d46644">current-context</span></i></b>

![[kubeconfig-3.png]]

- If you don't want to use the default <b><i><span style="color:#d46644">kubeconfig</span></i></b> file when running the <span style="color:red">kubectl config</span> command, you can pass in the <span style="color:red">--kubeconfig=</span> command with the location of the desired <b><i><span style="color:#d46644">kubeconfig</span></i></b> file

![[kubeconfig-4.png]]

- To update the <b><i><span style="color:#d46644">current-context</span></i></b>, run the <span style="color:red">kubectl config use-context</span> command with the name of the desired <b><i><span style="color:#d46644">context</span></i></b>
	- These changes will be reflected in the output of the command

![[kubeconfig-5.png]]

- Other changes can be made to the <b><i><span style="color:#d46644">kubeconfig</span></i></b> file using other variations of the <span style="color:red">kubectl config</span> command
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