#flashcards/kubernetes/cka/core-concepts

- A ==<b><span style="color:#d46644">ReplicaSet's</span></b>== purpose is to maintain a stable set of <b><span style="color:#d46644">replica</span></b> [[7 - Pods ✅|pods]] running at any given time.
	-  As such, it is often used to guarantee the availability of a specified number of identical [[7 - Pods ✅|pods]].

- A <b><span style="color:#d46644">ReplicaSet</span></b> is defined with fields, including
	- a <span style="color:red">selector</span> that specifies how to identify [[7 - Pods ✅|pods]] it can acquire
	- a number of <span style="color:red">replicas</span> indicating how many [[7 - Pods ✅|pods]] it should be maintaining
	- a <span style="color:red">template</span> specifying the data of new [[7 - Pods ✅|pods]] it should create to meet the number of <b><span style="color:#d46644">replicas</span></b> criteria

- A ==<b><span style="color:#d46644">ReplicaSet</span></b>== fulfills its purpose by creating and deleting [[7 - Pods ✅|pods]] as needed to reach the desired number
	- When a <b><span style="color:#d46644">ReplicaSet</span></b> needs to create new [[7 - Pods ✅|pods]], it uses its <span style="color:red">template</span>

- The ==<span style="color:#5c7e3e">apiVersion</span>== for <b><span style="color:#d46644">ReplicaSets</span></b> is different from that of <b><span style="color:#d46644">ReplicationControllers</span></b> as it is <i><span style="color:#477bbe">apps/v1</span></i>
	- If the <span style="color:#5c7e3e">apiVersion</span> is wrong, you will get an error because the <span style="color:#5c7e3e">Kubernetes apiVersion</span> (<span style="color:red">v1</span>) has no support for <b><span style="color:#d46644">ReplicaSets</span></b>

![[pods2.png]]

- The ==<span style="color:#5c7e3e">spec</span>== section of <b><span style="color:#d46644">ReplicaSets</span></b> look the same as that of <b><span style="color:#d46644">ReplicationController</span></b>

- There is one major difference between <b><span style="color:#d46644">ReplicationController</span></b> and <b><span style="color:#d46644">ReplicaSet</span></b> definitions which is that <b><span style="color:#d46644">ReplicaSets</span></b> have the ==<span style="color:red">selector</span>== property

- The ==<span style="color:red">selector</span>== property helps the <b><span style="color:#d46644">ReplicaSet</span></b> identify what [[7 - Pods ✅|pods]] fall under it
	- This is because, even though there is a <span style="color:red">template</span> section,  <b><span style="color:#d46644">ReplicaSets</span></b> can also manage [[7 - Pods ✅|pods]] that were not created as part of the <b><span style="color:#d46644">ReplicaSet</span></b> definition

- The ==<span style="color:red">selector</span>== is not a required field and, when skipped, is assumed to be the same as the [[1 - Labels & Selectors ✅|labels]] provided in the [[7 - Pods ✅|pod]] definition file

- The <span style="color:red">selector</span> is written in the form of two possible options:
	- <span style="color:red">matchLabels</span> which simply ==matches the [[1 - Labels & Selectors ✅|labels]]== specified under it to the [[1 - Labels & Selectors ✅|labels]] on the [[7 - Pods ✅|pods]]
	- <span style="color:red">matchExpressions</span> which is a more expressive [[1 - Labels & Selectors ✅|label selectors]] and supports ==set-based matching==

![[replicasets-1.png]]

![[pods3.png]]

- The <span style="color:red">kubectl create -f</span> command is the same with the proper file name

- The <span style="color:red">kubectl get replicasets|(rs) -f</span> command uses the keyword <b><span style="color:#d46644">ReplicaSets</span></b>


### Labels and Selectors

- The [[1 - Labels & Selectors ✅|labels]] in the ==<span style="color:#5c7e3e">metadata</span>== section (such as in [[7 - Pods ✅|pods]]) can be created as a filter for the <b><span style="color:#d46644">ReplicaSet</span></b>

- Both the key **and** value must be the same for the <span style="color:red">labels</span> and <span style="color:red">matchLabels</span> properties in order for the ==<b><span style="color:#d46644">ReplicaSets</span></b>== to know which [[7 - Pods ✅|pod]] to monitor

- The same concept as [[1 - Labels & Selectors ✅|labels and selectors]] are used in many other places across <span style="color:#5c7e3e">Kubernetes</span>

- The driving purpose behind keeping the ==<span style="color:red">template</span>== inside of the <b><span style="color:#d46644">ReplicaSet</span></b> definition is for creating new [[7 - Pods ✅|pods]] when existing [[7 - Pods ✅|pods]] fail or are removed

- There are multiple ways to <span style="color:#5c7e3e">scale</span> the <i><span style="color:#d46644">replicas</span></i>
	1. Update the number of <i><span style="color:#d46644">replicas</span></i> in the <b><span style="color:#d46644">ReplicaSet</span></b> definition files
		1. After a change/edit has been made to a file, use the <span style="color:red">kubectl replace|apply -f</span> command to apply those changes

		`kubectl apply -f <replica_definition_file_name>`

	1. Use the <span style="color:red">scale</span> command using the <span style="color:red">--replicas=</span> parameter to provide the new number of <i><span style="color:#d46644">replicas</span></i>, then specify the file with the <span style="color:red">-f</span> flag
		1. You can also use the name in the ***type name*** format

		`kubectl scale --replicas=<#_of_replicas> -f <replica_definition_file_name>`

					OR

		`kubectl scale --replicas=<#_of_replicas> replicaset <name_of_replicaset>`

		1. Keep in mind that if using the object ***type name*** format, the <i><span style="color:#d46644">replica</span></i> count will not be updated in the file (will not be persistent)

	1. <i><span style="color:#d46644">Replicas</span></i> can also be scaled based on load (advanced topic and will be covered later)

- It's important to note that the [[1 - Labels & Selectors ✅|labels]] used in the <span style="color:red">template.metadata.labels</span> section and the [[1 - Labels & Selectors ✅|labels]] used in the <span style="color:red">selector.matchLabels</span> of the <b><span style="color:#d46644">ReplicaSet</span></b> must match

- If more than the desired number of ==<i><span style="color:#d46644">replicas</span></i>== is running (with the same [[1 - Labels & Selectors ✅|labels]]), <span style="color:#5c7e3e">Kubernetes</span> will not allow this and terminate the newest created [[7 - Pods ✅|pods]].
	- This is because the number of <i><span style="color:#d46644">replicas</span></i> listed in the definition file is the desired number of running [[7 - Pods ✅|pods]] at all times (no more, no less)

From Kubernetes for Beginners - ReplicaSets & Replication Controllers
