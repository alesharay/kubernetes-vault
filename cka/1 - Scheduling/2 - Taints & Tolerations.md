- <b><span style="color:#d46644">Taints & Tolerations</span></b> set restrictions on what [[7 - Pods|pod]] can be [[4 - Kube Scheduler|scheduled]] on a [[0 - Core Concepts Intro|node]]
	(see the bug and repellent example)

- <b><span style="color:#d46644">Taints</span></b> are set on [[0 - Core Concepts Intro|nodes]]
- <b><span style="color:#d46644">Tolerations</span></b> are set on [[7 - Pods|pods]]

- <b><span style="color:#d46644">Taints & Tolerations</span></b> have nothing to do with security and intrusion

- By default, [[7 - Pods|pods]] have no <b><span style="color:#d46644">tolerations</span></b>

- Assume there are dedicated restrictions on a particular [[0 - Core Concepts Intro|node]] where only [[7 - Pods|pods]] that are part of a certain application can be placed on said [[0 - Core Concepts Intro|node]]

	
1. Apply a <b><span style="color:#d46644">taint</span></b> to the [[0 - Core Concepts Intro|node]]
		1. Unless specified otherwise, none of the [[7 - Pods|pods]] will be able to <b><span style="color:#d46644">tolerate</span></b> the specified <b><span style="color:#d46644">taint</span></b>
		2. This means no unwanted [[7 - Pods|pods]] will be placed on said [[0 - Core Concepts Intro|node]]
	1. To enable certain [[7 - Pods|pods]] to be placed on the [[0 - Core Concepts Intro|node]] with the <b><span style="color:#d46644">taint</span></b>, we must specify which [[7 - Pods|pods]] can <b><span style="color:#d46644">tolerate</span></b> that <b><span style="color:#d46644">taint</span></b>
		1. Do this by adding a <b><span style="color:#d46644">toleration</span></b> to the [[7 - Pods|pod]]
	1. Now, when the [[4 - Kube Scheduler|scheduler]] places the [[7 - Pods|pods]] on the [[0 - Core Concepts Intro|nodes]], the [[7 - Pods|pods]] that can <b><span style="color:#d46644">tolerate</span></b> specific <b><span style="color:#d46644">taints</span></b> on specific [[0 - Core Concepts Intro|nodes]] will be placed on the [[0 - Core Concepts Intro|nodes]]

- Use the <span style="color:red">kubectl taint nodes</span> command to place a <b><span style="color:#d46644">taint</span></b> on a [[0 - Core Concepts Intro|node]]. Specify the following:
	- [[0 - Core Concepts Intro|node]] name
	- <b><span style="color:#d46644">Taint</span></b> - key-value
	- <b><span style="color:#d46644">Taint effect</span></b> - placed on value separated by colon

		`kubectl taint nodes NODE_NAME key=value:taint-effect`

- The <b><span style="color:#d46644">taint</span></b> effect defines what happens to the [[7 - Pods|pod]] if they do not <b><span style="color:#d46644">tolerate</span></b> the <b><span style="color:#d46644">taint</span></b>

- There are three possible <b><span style="color:#d46644">taint-effects</span></b>

	- <b><span style="color:#d46644">NoSchedule</span></b>
		- The [[7 - Pods|pod]] cannot be [[4 - Kube Scheduler|scheduled]] on the [[0 - Core Concepts Intro|node]] if they don't <b><span style="color:#d46644">tolerate</span></b> the <b><span style="color:#d46644">taint</span></b>
	- <b><span style="color:#d46644">PreferNoSchedule</span></b>
		- The system will try to avoid placing the [[7 - Pods|pod]] on the [[0 - Core Concepts Intro|node]] but this is not a guarantee
	- <b><span style="color:#d46644">NoExecute</span></b>
		- New [[7 - Pods|pods]] will not be [[4 - Kube Scheduler|scheduled]] on the [[0 - Core Concepts Intro|node]] if they don't <b><span style="color:#d46644">tolerate</span></b> the <b><span style="color:#d46644">taint</span></b>
		- Existing [[7 - Pods|pods]] will be evicted from the [[0 - Core Concepts Intro|node]] if they don't <b><span style="color:#d46644">tolerate</span></b> the <b><span style="color:#d46644">taint</span></b>

		`kubectl taint nodes node1 app=blue:NoSchedule`

- To add <b><span style="color:#d46644">tolerations</span></b> to [[7 - Pods|pods]], add a <b><span style="color:#d46644">tolerations</span></b> property in the spec section of the [[7 - Pods|pod]] definition file

- The <b><span style="color:#d46644">tolerations</span></b> properties are as follows (all values of these properties should be in double quotes
	- Key
		- The <b><span style="color:#d46644">taint</span></b> key described on the [[0 - Core Concepts Intro|node]]
		- "<span style="color:#5c7e3e">app</span>" in the above example
	- Operator
		- The <b><span style="color:#d46644">taint</span></b> operator described on the [[0 - Core Concepts Intro|node]]
		- "<span style="color:#5c7e3e">Equal</span>" in the above example (must be spelled out)
		- The operator options are as follows
			- "<span style="color:#5c7e3e">Exists</span>"
				- No value should be specified
			- "<span style="color:#5c7e3e">Equal</span>"
				- A value should be specified

	- Value
		- The <b><span style="color:#d46644">taint</span></b> value described on the [[0 - Core Concepts Intro|node]]
		- "<span style="color:#5c7e3e">blue</span>" in the above example
	- Effect
		- The <b><span style="color:#d46644">taint</span></b> effect described on the [[0 - Core Concepts Intro|node]]
		- "<b><span style="color:#d46644">NoSchedule</span></b>" in the above example

![[tat-1.png]]

- The <b><span style="color:#d46644">tolerations</span></b> section is an <span style="color:#5c7e3e">array</span> of <span style="color:#5c7e3e">dictionaries</span>. The properties are all part of one <span style="color:#5c7e3e">dictionary</span>. There can be several if there are several <b><span style="color:#d46644">taints</span></b>

- For the <b><span style="color:#d46644">NoExecute</span></b> <b><span style="color:#d46644">taint effect</span></b> , once the [[0 - Core Concepts Intro|node]] <b><span style="color:#d46644">taint</span></b> takes affect, any existing [[7 - Pods|pods]] that are currently on the [[0 - Core Concepts Intro|node]] but cannot <b><span style="color:#d46644">tolerate</span></b> the <b><span style="color:#d46644">taint</span></b>, will be killed

- Keep in mind that <b><span style="color:#d46644">taint & tolerations</span></b> only restrict what pods can be placed on certain [[0 - Core Concepts Intro|nodes]]. They do not direct a [[7 - Pods|pod]] to a [[0 - Core Concepts Intro|node]]
	- It does not guarantee that a [[7 - Pods|pod]] with a particularly <b><span style="color:#d46644">toleration</span></b> will be placed on the [[0 - Core Concepts Intro|node]] that it <b><span style="color:#d46644">tolerates</span></b>.
		- This means, if the [[4 - Kube Scheduler|scheduler]] reaches other [[0 - Core Concepts Intro|nodes]] with different or no restrictions (<b><span style="color:#d46644">taints</span></b>) first , the [[7 - Pods|pod]] could be placed on another [[0 - Core Concepts Intro|node]]

- If you want to restrict a [[7 - Pods|pod]] to a certain [[0 - Core Concepts Intro|node]], this is called [[4 - Node Affinity|node affinity]] (will be discussed in another section)

- Notice that the [[4 - Kube Scheduler|scheduler]] does not [[4 - Kube Scheduler|schedule]] any [[7 - Pods|pods]] on the [[0 - Core Concepts Intro|master node]] (unless this is a single [[0 - Core Concepts Intro|node cluster]])
	- This is because when the Kubernetes [[0 - Core Concepts Intro|cluster]] is first setup, a <b><span style="color:#d46644">taint</span></b> is set on the [[0 - Core Concepts Intro|master node]] automatically that prevents any [[7 - Pods|pods]] from being [[4 - Kube Scheduler|scheduled]] on the [[0 - Core Concepts Intro|node]]

- To see the <b><span style="color:#d46644">taint</span></b> on the [[0 - Core Concepts Intro|master node]], run the <span style="color:red">kubectl describe node</span> command with kubemaster [[0 - Core Concepts Intro|node]]
	- Grep "<b><span style="color:#d46644">taint</span></b>" for easy [[0 - Core Concepts Intro|node]]

		`kubectl describe node kube-master | grep -i taint`

- To remove a <b><span style="color:#d46644">taint</span></b> from a [[0 - Core Concepts Intro|node]], use the same syntax (check to see if the <b><span style="color:#d46644">taint</span></b> exists) but with a " - " at the end (no space)

	Using the above example

		`kubectl taint nodes node1 app=blue:NoSchedule-`