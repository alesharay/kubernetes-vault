link- There are many candidates for <b><span style="color:#d46644">backups</span></b> in a <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]]
	- <i><span style="color:#477bbe">Resource</span></i> definition files
	- The [[1 - ETCD ✅|etcd]] store
	- [[4 - Persistent Volumes|Persistent Volumes]]

![[backup-1.png]]

### <span style="color:#477bbe">Resource</span> config file <b><span style="color:#d46644">backups</span></b>

- A good practice is to store <i><span style="color:#477bbe">resource</span></i> definition files on <span style="color:#5c7e3e">source code repositories</span>

- The <span style="color:#5c7e3e">source code repositories</span> should be configured with the right <b><span style="color:#d46644">backup</span></b> solutions

- With managed/public <span style="color:#5c7e3e">source code repositories</span>, such as <span style="color:#5c7e3e">GitHub</span> and <span style="color:#5c7e3e">GitLab</span>, <b><span style="color:#d46644">backup</span></b> solutions are automatically available
	- With this method, even if you lose the entire [[0 - Core Concepts Intro ✅|cluster]], you can bring <b><span style="color:#d46644">backup</span></b> the application by simply applying the configuration files

- If <i><span style="color:#477bbe">resources</span></i> were created imperatively, the better approach to backing up <i><span style="color:#477bbe">resource</span></i> configuration is to query the [[2 - Kube API server ✅|kube-apiserver]]

- Query the [[2 - Kube API server ✅|kube-apiserver]] using <span style="color:#5c7e3e">kubectl</span> or by accessing the [[2 - Kube API server ✅|API server]] directly and saving all <i><span style="color:#477bbe">resource</span></i> configurations, so that all objects created on the [[0 - Core Concepts Intro ✅|cluster]] have a copy

- One of the commands that can be used in a <b><span style="color:#d46644">backup</span></b> script is to get all [[7 - Pods|pods]], [[9 - Deployments|deployments]], and [[10 - Services|services]] in all [[11 - Namespaces|na]] using the <span style="color:red">kubectl get all --all-namespaces</span> command and redirect the output to a YAML file

![[backup-2.png]]

- There are many other <i><span style="color:#477bbe">resource</span></i> groups that must be considered

- <b><span style="color:#d46644">Backup</span></b> solutions don't have to be created manually, there are tools like ARK (now called Velero by Heptio) that can do this as well

### [[1 - ETCD ✅|Etcd]] <b><span style="color:#d46644">backup</span></b>

- [[1 - ETCD ✅|Etcd]] stores information about the state of the [[0 - Core Concepts Intro ✅|cluster]]
	- This means information about the [[0 - Core Concepts Intro ✅|cluster]], the [[0 - Core Concepts Intro ✅|nodes]], and every other <i><span style="color:#477bbe">resource</span></i> created within the [[0 - Core Concepts Intro ✅|cluster]] are stored here

- Instead of backing up <i><span style="color:#477bbe">resource</span></i> config files, you can <b><span style="color:#d46644">backup</span></b> the [[1 - ETCD ✅|etcd]] server itself

- As we have seen, the [[1 - ETCD ✅|etcd]] store is hosted on the [[0 - Core Concepts Intro ✅|master node]] and while configuring [[1 - ETCD ✅|etcd]], we specified a location where all of the data would be stored

![[backup-3.png]]

- [[1 - ETCD ✅|Etcd]] comes with a built-in snapshot solution
	- You can take a snapshot of the [[1 - ETCD ✅|etcd]] database by using the <span style="color:#5c7e3e">etcdctl</span> utility's <span style="color:red">etcdctl snapshot save</span> command
		- Give the snapshot a name

![[backup-4.png]]

- A snapshot file is created, with the provided name, in the directory where the command was run
	- If you want the snapshot to be created in another location, specify the full path

- You can view the status of the snapshot using the <span style="color:red">etcdctl snapshot status</span> command

![[backup-5.png]]

------------------------------------------------------------------------------------------------------

### To restore the [[1 - ETCD ✅|etcd]] store from a snapshot (<b><span style="color:#d46644">backup</span></b>) at a later point

1. Stop the [[2 - Kube API server ✅|kube-apiserver]] as the restore process will require you to restart the [[1 - ETCD ✅|etcd]] store and the [[2 - Kube API server ✅|kube-apiserver]] depends on it
2. Run the <span style="color:red">etcdctl snapshot restore</span> command with the path set to the path of the <b><span style="color:#d46644">backup</span></b> file

	![[backup-6.png]]

	- When [[1 - ETCD ✅|etcd]] restores from a <b><span style="color:#d46644">backup</span></b>, it initializes a new [[0 - Core Concepts Intro ✅|cluster]] configuration and configures the members of [[1 - ETCD ✅|etcd]] as new members to a new [[0 - Core Concepts Intro ✅|cluster]]
	- This is to prevent a new member from accidentally joining an existing [[0 - Core Concepts Intro ✅|cluster]]

3. During a restore, you must specify a new [[0 - Core Concepts Intro ✅|cluster]] token and the same initial [[0 - Core Concepts Intro ✅|cluster]] configuration options specified in the original configuration file
	1. On running this command, a new data directory is created

	![[backup-7.png]]

4. Then configure the [[1 - ETCD ✅|etcd]] configuration file to use the new [[0 - Core Concepts Intro ✅|cluster]]-token and data directory

5. Then reload the [[10 - Services|services]] daemon and restart the [[1 - ETCD ✅|etcd]] service

![[backup-8.png]]

6. Your [[0 - Core Concepts Intro ✅|cluster]] should now be back in the original state

7. Remember to specify the [[3.1 - Certification Creation|certificate]] files for [[1 - Authentication|authentication]]
	1. Specify the endpoint to the [[1 - ETCD ✅|etcd]] store and the [[3.1 - Certification Creation|ca certificate]], the [[3.1 - Certification Creation|etcd server certificate]], and the key

![[backup-9.png]]

------------------------------------------------------------------------------------------------------

- To make use of <span style="color:#5c7e3e">etcdctl</span> for tasks such as <b><span style="color:#d46644">backup</span></b> and restore, make sure you set the <span style="color:#5c7e3e">ETCDCTL_API </span>to 3

		export ETCDCTL_API=3

- Use the `etcdctl snapshot save -h` option and keep note of the "mandatory" global options

- If you get an error that says "no help topic for `snapshot`", this means that you haven't set the <span style="color:#5c7e3e">ETCDCTL_API </span>version

### Practice Problems

- What is the version of etcd running on the cluster?

		kubectl describe pod etcd-controlplane -n kube-system | grep -i image

	(3.5.1)

- At what address can you reach the etcd cluster from the controlplane node? (check the etcd service configuration in the etcd pod)

		kubectl describe pod etcd-controlplane -n kube-system | grep -i listen-client

([https://[127.0.0.1]:2379](https://[127.0.0.1]:2379))

- Where is the etcd server certificate file located? (note this path as it will be needed later)

		kubectl describe pod etcd-controlplane -n kube-system | grep -i cert-file

	(/etc/kubernetes/pki/etcd/server.crt)

- Where is the etcd server key file located? (note this path as it will be needed later)

		kubectl describe pod etcd-controlplane -n kube-system | grep -i key

	(/etc/kubernetes/pki/etcd/server.key)

- Where is the etcd ca certificate file located? (note this path as it will be needed later)

		kubectl describe pod etcd-controlplane -n kube-system | grep -i trusted-ca-file

	(/etc/kubernetes/pki/etcd/ca.crt)

- What is the data directory for the ectd cluster

		kubectl describe pod etcd-controlplane -n kube-system | grep -i data-dir

	(/var/lib/etcd)

(the files located here are the files created by etcd to store information)

- Take a snapshot of the etcd database using the built-in snapshot functionality.

	(store the <b><span style="color:#d46644">backup</span></b> file at location /opt/snapshot-pre-boot.db)

		export ETCDCTL_API=3
		etcdctl snapshot save -h | grep -i tls
		etcdctl snapshot save \
		--endpoints=https://[127.0.0.1]:2379 \
		--cacert=/etc/kubernetes/pki/etcd/ca.crt \
		--cert=/etc/kubernetes/pki/etcd/server.crt \
		--key=/etc/kubernetes/pki/etcd/server.key \
		/opt/snapshot-pre-boot.db

- Check the status of the applications on the cluster

		kubectl get all

- Restore the original state of the cluster using the <b><span style="color:#d46644">backup</span></b> file

		etcdctl --data-dir /var/lib/etcd-from-backup \ snapshot restore /opt/snapshot-pre-boot.db

	(edit the etcd manifest file (/etc/kubernetes/manifests/etcd.yaml) with the new host-path from /var/lib/etcd to  /var/lib/etcd-from-backup)