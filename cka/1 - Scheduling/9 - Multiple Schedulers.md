- What if, when the scheduler is scheduling [[7 - Pods|pods]] to nodes, none of the taints & tolerations or nodeAffinity & nodeSelector rules satisfies the needs
	- What if you want to use your own scheduling algorithm to place [[7 - Pods|pods]] on nodes

- You can write your own Kubernetes scheduler program, package it and deploy it as the default scheduler or as an additional scheduler in the [[0 - Core Concepts Intro|cluster]]

- Your Kubernetes [[0 - Core Concepts Intro|cluster]] can have multiple schedulers at the same time

- When creating [[7 - Pods|pods]] or deployments, you can instruct Kubernetes to have the [[7 - Pods|pods]] scheduled by a specific scheduler

- When deploying a scheduler manually, by downloading the scheduler binary and running it as a service (OS service - not Kubernetes service), one of the options is the --scheduler-name option
	- If this is not set, it will use the default scheduler

- To deploy an additional scheduler, you can use the same kube-scheduler binary
	- or use one that you may have built yourself (this would make the most sense)

![[mult-schedulers-1.png]]

NOTE: be sure to note the name you give the scheduler as you will be using it in the [[7 - Pods|pod]] or deployment definition files later

- The <b><i><span style="color:#5c7e3e">kubeadm</span></i></b> tool deploys the scheduler as a [[7 - Pods|pod]]
	- The command property shows the associated options to start the scheduler

![[mult-schedulers-2.png]]

* To make an additional scheduler, you can copy the manifest file and change the name of the scheduler

![[mult-schedulers-3.png]]

- An important option to focus on is the --leader-elect option.
	- This option is used when you have multiple copies of the scheduler running on different master nodes (this is a high availability setup where there are multiple master nodes)
	- If multiple copies of the same scheduler are running on the different nodes, only one can be active at a time

- This is where the --leader-elect option helps in choosing a scheduler who will lead scheduling activities

- If you don't have multiple master nodes, the --leader-elect options should be "false".

- If you do have multiple master nodes, you can pass in an additional parameter to set a "lock object" name (DEPRECATED: Will be removed in favor of leader-elect-resource-name)
	- This is to differentiate the new custom scheduler from the default during the leader election process

![[mult-schedulers-4.png]]

- Create the [[7 - Pods|pod]] using the `kubectl create` command and run the `kubectl get [[7 - Pods|pod]]s` command in the kube-system namespace to see the schedulers running

![[mult-schedulers-5.png]]

- The next step is to configure a new [[7 - Pods|pod]] or deployment to use the new scheduler

- In the definition file, add a field called schedulerName and specify the name of the new scheduler

![[mult-schedulers-6.png]]

- By default, if this property is not defined, it automatically uses the default scheduler

- To know which scheduler picked up the [[7 - Pods|pod]] creation responsibilities, view the events using the `kubectl get events` command
	- This lists all of the events in the current namespace

![[mult-schedulers-7.png]]

- You can also view the logs of the scheduler using the `kubectl logs` command

![[mult-schedulers-8.png]]

#### Practice Problems

- What is the name of the [[7 - Pods|pod]] that deploys the default kubernetes scheduler in this environment?

        kubectl get [[7 - Pods|pod]]s -A