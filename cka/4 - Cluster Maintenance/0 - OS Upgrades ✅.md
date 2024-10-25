#flashcards/kubernetes/cka/cluster-maintenance

- When a [[0 - Core Concepts Intro ✅|cluster]] or [[0 - Core Concepts Intro ✅|node]] goes down, the [[7 - Pods ✅|pods]] on the [[0 - Core Concepts Intro ✅|node]] are not available
	- Depending on how you deployed the [[7 - Pods ✅|pods]], that could impact your <i><span style="color:#477bbe">users</span></i>

- If a [[0 - Core Concepts Intro ✅|node]] that went down comes back up immediately, then the <span style="color:#5c7e3e">kubectl</span> process starts and the [[7 - Pods ✅|pods]] come back online

- If a [[0 - Core Concepts Intro ✅|node]] that went down stays down for more than 5 minutes, the [[7 - Pods ✅|pods]] are terminated from that [[0 - Core Concepts Intro ✅|node]]

- If the [[7 - Pods ✅|pods]], on a [[0 - Core Concepts Intro ✅|node]] that has been down for more than 5 minutes, were part of a [[8 - ReplicaSets ✅|replicaSet]], then they are recreated on other [[0 - Core Concepts Intro ✅|nodes]]

- If the [[7 - Pods ✅|pods]], on a [[0 - Core Concepts Intro ✅|node]] that has been down for more than 5 minutes, were not part of a [[8 - ReplicaSets ✅|replicaSet]], then they will be lost forever

- The time the  [[0 - Core Concepts Intro ✅|master nodes]] wait for [[7 - Pods ✅|pods]] to come back online is known as the <b><span style="color:#d46644">pod eviction timeout</span></b> and is set on the [[3 - Kube Controller Manager ✅|controller manager]] with a default value of 5 minutes

	![[osupgrades-1.png]]

	- The [[0 - Core Concepts Intro ✅|master node]] waits for up to 5 minutes before considering the [[0 - Core Concepts Intro ✅|node]] dead

- When a [[0 - Core Concepts Intro ✅|node]] comes back online after the <b><span style="color:#d46644">pod eviction timeout</span></b>, it comes up blank without any [[7 - Pods ✅|pods]] scheduled

- If you have maintenance tasks to be performed on a [[0 - Core Concepts Intro ✅|node]], you know that the workloads running have other [[8 - ReplicaSets ✅|replicas]], it's okay that they go down for a short period of time, and you're sure the [[0 - Core Concepts Intro ✅|node]] will come back online within 5 minutes, then you can make a quick <b><span style="color:#d46644">upgrade</span></b> and reboot

- If you do not know for sure if a [[0 - Core Concepts Intro ✅|node]] will be back online within 5 minutes or you cannot say for sure it will come back online at all, there is a safer way to make your <b><span style="color:#d46644">upgrades</span></b>

- To make your <b><span style="color:#d46644">upgrades</span></b> safely, you can purposefully <b><span style="color:#d46644">drain</span></b> the [[0 - Core Concepts Intro ✅|node]] of all the workloads so that the workloads are terminated on the [[0 - Core Concepts Intro ✅|nodes]] that they're on and are recreated on other [[0 - Core Concepts Intro ✅|nodes]]
	- The [[0 - Core Concepts Intro ✅|node]] is also <b><span style="color:#d46644">cordoned</span></b> (marked as <span style="color:#5c7e3e">unschedulable</span>) so that no other [[7 - Pods ✅|pods]] can be scheduled on it until you specifically remove the restriction

- Once the [[7 - Pods ✅|pods]] are safely on other [[0 - Core Concepts Intro ✅|nodes]], you can reboot the original [[0 - Core Concepts Intro ✅|node]]

- When a [[0 - Core Concepts Intro ✅|node]] has been rebooted and comes back online, it is still originally <b><span style="color:#d46644">cordoned</span></b> (<span style="color:#5c7e3e">unschedulable</span>)
	- You then need to <b><span style="color:#d46644">uncordon</span></b> it so that [[7 - Pods ✅|pods]] can be scheduled again

- After a [[0 - Core Concepts Intro ✅|node]] has been <b><span style="color:#d46644">uncordoned</span></b>, the [[7 - Pods ✅|pods]] that were originally on the [[0 - Core Concepts Intro ✅|node]] don't automatically come back on it
	- Only deleted [[7 - Pods ✅|pods]] that have been rolled back or new [[7 - Pods ✅|pods]] will be created on the [[0 - Core Concepts Intro ✅|node]]

- You can also manually <b><span style="color:#d46644">cordon</span></b> a [[0 - Core Concepts Intro ✅|node]] which will mark it as <span style="color:#5c7e3e">unschedulable</span>
	- This does not <b><span style="color:#d46644">drain</span></b> or move the [[7 - Pods ✅|pods]], it simply makes sure new [[7 - Pods ✅|pods]] are not able to be scheduled on that [[0 - Core Concepts Intro ✅|node]]

![[osupgrades-2.png]]

### Practice Problems

- How many applications do you see hosted on the cluster

		kubectl get deployments

- How many nodes are the applications posted on?

		kubectl get pods -o wide

- We need to take a node01 down for maintenance; empty the node of all applications and mark it as unschedulable

		kubectl drain node01 --ignore-daemonsets

- Configure node01 to be schedulable again

		kubectl uncordon node01

- Mark node01 as unschedulable

		kubectl cordon node01