- When a cluster or node goes down, the pods on the node are not available

- Depending on how you deployed the pods, that could impact your users

- If a node that went down comes back up immediately, then the kubectl process starts and the pods come back online

- If a node that went down stays down for more than 5 minutes, the pods are terminated from that node

- If the pods, on a node that's been down for more than 5 minutes, were part of a replicaSet, then they are recreated on other nodes

- If the pods, on a node that's been down for more than 5 minutes, were not part of a replicaSet, then they will be lost forever

- The time the master nodes wait for pods to come back online is known as the pod eviction timeout and is set on the controller manager with a default value of 5 minutes

![[osupgrades-1.png]]

- The master node waits for up to 5 minutes before considering the node dead

- When a node comes back online after the pod eviction timeout, it comes up blank without any pods scheduled

- If you have maintenance tasks to be performed on a node, you know that the workloads running have other replicas, it's okay that they go down for a short period of time, and you're sure the node will come back online within 5 minutes, you can make a quick upgrade and reboot

- If you do not know for sure if a node will be back online within 5 minutes or you cannot say for sure it will come back online at all, there is a safer way to make your upgrades

- To make your upgrades safely, you can purposefully drain the node of all the workloads so that the workloads are terminated on the nodes that they're on and are recreated on other nodes

- The node is also cordoned (marked as unschedulable) so that no other pods can be scheduled on it until you specifically remove the restriction

- Once the pods are safely on other nodes, you can reboot the original node

- When a node has been rebooted and comes back online, it is still originally cordoned (unschedulable)

- You then need to uncordon it so that pods can be scheduled again

- After a node has been uncordoned, the pods that were originally on the node don't automatically come back on it

- Only deleted pods that have been rolled back or new pods will be created on the node

- You can also manually cordon a node which will mark it as unschedulable

- This does not drain or move the pods, it simply makes sure new pods are not able to be scheduled on that node

![[osupgrades-2.png]]

### Practice Problems

- How many applications do you see hosted on the cluster

		kubectl get deployments

- How many nodes are the applications posted on?

		kubectl get pods -o wide

- We need to take a node01 down for maintenance; empty the node of all applications and mark it as un-schedulable

		kubectl drain node01 --ignore-daemonsets

- Configure node01 to be schedulable again

		kubectl uncordon node01

- Mark node01 as unschedulable

		kubectl cordon node01