- A <b><span style="color:#d46644">ReplicaSet's</span></b> purpose is to maintain a stable set of replica [[7 - Pods|pods]] running at any given time.
	-  As such, it is often used to guarantee the availability of a specified number of identical [[7 - Pods|pods]].

- A <b><span style="color:#d46644">ReplicaSet</span></b> is defined with fields, including
	- a [[1 - Labels & Selectors|selector]] that specifies how to identify [[7 - Pods|Pods]] it can acquire
	- a number of <i><span style="color:#477bbe">replicas</span></i> indicating how many [[7 - Pods|pods]] it should be maintaining
	- a [[7 - Pods|pod]] <span style="color:#5c7e3e">template</span> specifying the data of new [[7 - Pods|pods]] it should create to meet the number of <i><span style="color:#477bbe">replicas</span></i> criteria

- A <b><span style="color:#d46644">ReplicaSet</span></b> fulfills its purpose by creating and deleting [[7 - Pods|pods]] as needed to reach the desired number.
	- When a <b><span style="color:#d46644">ReplicaSet</span></b> needs to create new [[7 - Pods|pods]], it uses its [[7 - Pods|pod]] <span style="color:#5c7e3e">template</span>.

- The <i><span style="color:#477bbe">apiVersion</span></i> for <b><span style="color:#d46644">ReplicaSets</span></b> is different from that of <b><span style="color:#d46644">ReplicationControllers</span></b> as it is <i><span style="color:#477bbe">apps/v1</span></i>
	- If the <i><span style="color:#477bbe">apiVersion</span></i> is wrong, you will get an error because the specified k8s <i><span style="color:#477bbe">apiVersion</span></i> (v1) has no support for <b><span style="color:#d46644">ReplicaSets</span></b>

- The <i><span style="color:#477bbe">spec</span></i> section of <b><span style="color:#d46644">ReplicaSets</span></b> look the same as that of <b><span style="color:#d46644">ReplicationController</span></b>

- There is one major difference between <b><span style="color:#d46644">ReplicationController</span></b> and <b><span style="color:#d46644">ReplicaSet</span></b> definitions which is that <b><span style="color:#d46644">ReplicaSets</span></b> have the [[1 - Labels & Selectors|selector]] property

- The [[1 - Labels & Selectors|selector]] property helps the <b><span style="color:#d46644">ReplicaSet</span></b> identify what [[7 - Pods|pods]] fall under it
	- This is because, even though there is a [[7 - Pods|pod]] template section,  <b><span style="color:#d46644">ReplicaSets</span></b> can also manage [[7 - Pods|pods]] that were not created as part of the <b><span style="color:#d46644">ReplicaSet</span></b> definition

- The [[1 - Labels & Selectors|selector]] is not a required field and, when skipped, is assumed to be the same as the labels provided in the [[7 - Pods|pod]] definition file

- The [[1 - Labels & Selectors|selector]] is written in the form of two possible options:
	- <i><span style="color:#477bbe">matchLabels</span></i> which simply matches the [[1 - Labels & Selectors|labels]] specified under it to the [[1 - Labels & Selectors|labels]] on the [[7 - Pods|pods]]
	- <i><span style="color:#477bbe">matchExpressions</span></i> (we will cover later)

![[replicasets-1.png]]

- The <span style="color:red">kubectl create -f</span> command is the same with the proper file name

- The <span style="color:red">kubectl get replicasets/(rs) -f</span> command uses the keyword <b><span style="color:#d46644">ReplicaSets</span></b>

Labels and Selectors

- The labels in the metadata sections (such as in [[7 - Pods|pods]]) can be created as a filter for the <b><span style="color:#d46644">ReplicaSet</span></b>

- Both the key and value must be the same for the [[1 - Labels & Selectors|labels]] and <i><span style="color:#477bbe">matchLabels</span></i> properties in order for the <b><span style="color:#d46644">ReplicaSets</span></b> to know which [[7 - Pods|pod]] to monitor

- The same concept as [[1 - Labels & Selectors|labels]] and [[1 - Labels & Selectors|selectors]] are used in many other places across k8s

- The driving purpose behind keeping the [[7 - Pods|pod]] template inside of the <b><span style="color:#d46644">ReplicaSet</span></b> definition is for creating new [[7 - Pods|pods]] when existing [[7 - Pods|pods]] fail or are removed

- There are multiple ways to <span style="color:#5c7e3e">scale</span> the <i><span style="color:#477bbe">replicas</span></i>
	1. <b><span style="color:#5c7e3e">Update</span></b> the number of <i><span style="color:#477bbe">replicas</span></i> in the <b><span style="color:#d46644">ReplicaSet</span></b>/<b><span style="color:#d46644">ReplicationController</span></b> definition files
		1. After a change/edit has been made to a file, use the <span style="color:red">kubectl replace -f</span> command (similar to create and apply) to apply those changes

		`kubectl apply -f <replica_definition_file_name>`

	1. Use the <i><span style="color:#477bbe">scale</span></i> command using the " <span style="color:red">--replicas=</span> " parameter to provide the new number of <i><span style="color:#477bbe">replicas</span></i>, then specify the file with the -f flag
		1. You can also use the name in the type name format

		`kubectl scale --replicas=<#_of_replicas> -f <replica_definition_file_name>`

					OR

		`kubectl scale --replicas=<#_of_replicas> replicaset <name_of_replicaset>`

		1. Keep in mind that if using the object type name format, the <i><span style="color:#477bbe">replica</span></i> count will not be updated in the file (will not be persistent)

	1. <i><span style="color:#477bbe">Replicas</span></i> can also be scaled based on load (advanced topic and will be covered later)

- It's important to note that the [[1 - Labels & Selectors|labels]] used on the [[7 - Pods|pod]] template and the [[1 - Labels & Selectors|labels]] used in the <i><span style="color:#477bbe">matchSelectors</span></i> of the <b><span style="color:#d46644">ReplicaSet</span></b> must match

- If more than the desired number of <i><span style="color:#477bbe">replicas</span></i> is running (with the same [[1 - Labels & Selectors|labels]]), k8s will not allow this and terminate the newest created [[7 - Pods|pods]].
	- This is because the number if <i><span style="color:#477bbe">replicas</span></i> listed in the definition file is the desired number of running [[7 - Pods|pods]] at all times (no more no less)

[From Kubernetes for Beginners - ReplicaSets & Replication Controllers](onenote:Kubernetes%20for%20Beginners.one#RepliaSets%20and%20Replication%20Controllers&section-id={5D5A45D8-45DB-1442-8EBF-F2131933F0D4}&page-id={CB654E3B-5584-2548-B79B-DE46A4557F20}&end&base-path=https://d.docs.live.net/a8ff567768035d78/Documents/Kubernetes)

