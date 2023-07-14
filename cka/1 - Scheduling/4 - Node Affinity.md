- <b><span style="color:#d46644">nodeAffinity</span></b> ensures that [[7 - Pods|pods]] are placed on particular [[0 - Core Concepts Intro|nodes]]
	- We are provided with advanced capabilities to limit [[7 - Pods|pods]] being placed on specific [[0 - Core Concepts Intro|nodes]]

- Although [[3 - Node Selectors|nodeSelector]] and <b><span style="color:#d46644">nodeAffinity</span></b> technically do the same thing, <b><span style="color:#d46644">nodeAffinity</span></b> configuration is much more complex

- <b><span style="color:#d46644">nodeAffinity</span></b> config looks as follows
	- <b><span style="color:#d46644">affinity</span></b>
		- As a child of <b><span style="color:#d46644">affinity</span></b> is <b><span style="color:#d46644">nodeAffinity</span></b>
			- As a child of <b><span style="color:#d46644">nodeAffinity</span></b> is a "sentence-like" structure called the <b><span style="color:#d46644">nodeAffinity</span></b> <span style="color:#5c7e3e">type</span>
				- As a child of the <b><span style="color:#d46644">nodeAffinity</span></b> <span style="color:#5c7e3e">type</span> are the rules used to make the determinations
					- If <b><span style="color:#d46644">nodeAffinity</span></b> <span style="color:#5c7e3e">type</span> is <b><span style="color:#d46644">requiredDuringSchedulingIgnoredDuringExecution</span></b> then the child of this type is [[3 - Node Selectors|nodeSelectorTerms]]
						- Under which is [[1 - Labels & Selectors|matchExpressions]]
					- If <b><span style="color:#d46644">nodeAffinity</span></b> <span style="color:#5c7e3e">type</span> is <b><span style="color:#d46644">preferredDuringSchedulingIgnoredDuringExecution</span></b> then the child is preference
						- Under which is [[1 - Labels & Selectors|matchExpressions]]

![[node-affinity-1.png]]

- In this case, values is a list of all the values that fall under the specific operator

- The operator options include:
	- <span style="color:#5c7e3e">in</span>
	- <span style="color:#5c7e3e">NotIn</span>
	- <span style="color:#5c7e3e">Exists</span>
	- <span style="color:#5c7e3e">DoNotExist</span>
	- <span style="color:#5c7e3e">Gt</span>
	- <span style="color:#5c7e3e">Lt</span>

- If <b><span style="color:#d46644">nodeAffinity</span></b> cannot match a [[0 - Core Concepts Intro|node]] to a given expression, this is where the <b><span style="color:#d46644">nodeAffinity</span></b> type comes in

- The type of <b><span style="color:#d46644">nodeAffinity</span></b> defines the behavior of the [[4 - Kube Scheduler|scheduler]]  with respect to <b><span style="color:#d46644">nodeAffinity</span></b> and the stages in the lifecycle of the [[7 - Pods|pod]]

- <b><span style="color:#d46644">nodeAffinity</span></b> types:
	- Available for current <span style="color:#5c7e3e">Kubernetes</span> versions
		- <b><span style="color:#d46644">requiredDuringSchedulingIgnoredDuringExecution</span></b>
			- The [[4 - Kube Scheduler|scheduler]] can't schedule the [[7 - Pods|pod]] unless the rule is met. This functions like [[3 - Node Selectors|nodeSelector]], but with a more expressive syntax

		- <b><span style="color:#d46644">preferredDuringSchedulingIgnoredDuringExecution</span></b>
			- The [[4 - Kube Scheduler|scheduler]]  tries to find a [[0 - Core Concepts Intro|node]] that meets the rule. If a matching [[0 - Core Concepts Intro|node]] is not available, the [[4 - Kube Scheduler|scheduler]]  still schedules the [[7 - Pods|pod]]

		Note: In the preceding types, <b><span style="color:#d46644">ignoredDuringExecution</span></b> means that if the [[0 - Core Concepts Intro|node]] [[1 - Labels & Selectors|labels]] change after <span style="color:#5c7e3e">Kubernetes</span> schedules the [[7 - Pods|pod]], the [[7 - Pods|pod]] continues to run.

- When [[7 - Pods|pods]] are created, these rules are considered when placing the [[7 - Pods|pods]] on the correct [[0 - Core Concepts Intro|nodes]]

- The type of <b><span style="color:#d46644">nodeAffinity</span></b> comes into play in situations where you forget to [[1 - Labels & Selectors|label]] the [[0 - Core Concepts Intro|node]]

- There are two states in the lifecycle of a [[7 - Pods|pod]] when considering <b><span style="color:#d46644">nodeAffinity</span></b>:
	- during scheduling
					- The state where a [[7 - Pods|pod]] does not exist and is created for the first time
					- The [[4 - Kube Scheduler|scheduler]]  will mandate that the [[7 - Pods|pod]] be placed on a [[0 - Core Concepts Intro|node]] with given <b><span style="color:#d46644">affinity</span></b> rules
		- required
			- If it cannot find a matching [[0 - Core Concepts Intro|node]], the [[7 - Pods|pod]] will not be scheduled
			- This type is used when the placement of the [[7 - Pods|pod]] is crucial
		- preferred
			- In cases where a matching [[0 - Core Concepts Intro|node]] is not found, the [[4 - Kube Scheduler|scheduler]]  will simply ignore <b><span style="color:#d46644">nodeAffinity</span></b> rules and place the [[7 - Pods|pod]] on any available [[0 - Core Concepts Intro|node]]
			- This type is used when the placement of the [[7 - Pods|pod]] is less important
	- during execution
		- This is the state where a [[7 - Pods|pod]] has been running and a change is made in the environment that affects <b><span style="color:#d46644">nodeAffinity</span></b> (such as a change in the [[1 - Labels & Selectors|label]] of the [[0 - Core Concepts Intro|node]])
		- [[7 - Pods|pods]] will continue to run and any changes to <b><span style="color:#d46644">nodeAffinity</span></b> will not affect them