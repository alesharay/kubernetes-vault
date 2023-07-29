- There are many candidates for backups in a Kubernetes cluster

- Resource definition files
- The etcd store
- Persistent volumes

![[backup-1.png]]

### Resource config file backups

- A good practice is to store resource definition files on source code repositories

- The source code repositories should be configured with the right backup solutions

- With managed/public source code repositories, such as GitHub and GitLab, backup solutions are automatically available

- With this method, even if you lose the entire cluster, you can bring back up the application by simply applying the configuration files

- If resources were created imperatively, the better approach to backing up resource configuration is to query the kube-apiserver

- Query the kube-apiserver using kubectl or by accessing the API server directly and saving all resource configurations, so that all objects created on the cluster have a copy

- One of the commands that can be used in a backup script is to get all pods, deployments, and services in all namespaces using the <span style="color:red">kubectl get all --all-namespaces</span> command and redirect the output to a YAML file

![[backup-2.png]]

- There are many other resource groups that must be considered

- Backup solutions don't have to be created manually, there are tools like ARK (now called Velero by Heptio) that can do this as well

### Etcd backup

- Etcd stores information about the state of the cluster

- This means information about the cluster, the nodes, and every other resource created within the cluster are stored here

- Instead of backing up resource config files, you can backup the etcd server itself

- As we have seen, the etcd store is hosted on the master node and while configuring etcd, we specified a location where all of the data would be stored

![[backup-3.png]]

- Etcd comes with a built-in snapshot solution

- You can take a snapshot of the etcd database by using the etcdctl utility's <span style="color:red">etcdctl snapshot save</span> command

- Give the snapshot a name

![[backup-4.png]]

- A snapshot file is created, with the provided name, in the directory where the command was run

- If you want the snapshot to be created in another location, specify the full path

- You can view the status of the snapshot using the <span style="color:red">etcdctl snapshot status</span> command

![[backup-5.png]]

------------------------------------------------------------------------------------------------------

### To restore the etcd store from a snapshot (backup) at a later point

1. Stop the kube-apiserver as the restore process will require you to restart the ETCD store and the kube-apiserver depends on it
2. Run the <span style="color:red">etcdctl snapshot restore</span> command with the path set to the path of the backup file

![[backup-6.png]]

- When etcd restores from a backup, it initializes a new cluster configuration and configures the members of etcd as new members to a new cluster
- This is to prevent a new member from accidentally joining an existing cluster

1. During a restore, you must specify a new cluster token and the same initial cluster configuration options specified in the original configuration file

1. On running this command, a new data directory is created

![[backup-7.png]]

1. Then configure the etcd configuration file to use the new cluster-token and data directory

1. Then reload the service daemon and restart the etcd service

![[backup-8.png]]

1. Your cluster should now be back in the original state

1. Remember to specify the certificate files for authentication

1. Specify the endpoint to the etcd store and the ca certificate, the etcd server certificate, and the key

![[backup-9.png]]

------------------------------------------------------------------------------------------------------

- To make use of etcdctl for tasks such as backup and restore, make sure you set the ETCDCTL_API to 3

		export ETCDCTL_API=3

- Use the `etcdctl snapshot save -h` option and keep note of the "mandatory" global options

- If you get an error that says "no help topic for `snapshot`", this means that you haven't set the ETCDCTL_API version

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

	(store the backup file at location /opt/snapshot-pre-boot.db)

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

- Restore the original state of the cluster using the backup file

		etcdctl --data-dir /var/lib/etcd-from-backup \ snapshot restore /opt/snapshot-pre-boot.db

	(edit the etcd manifest file (/etc/kubernetes/manifests/etcd.yaml) with the new host-path from /var/lib/etcd to  /var/lib/etcd-from-backup)