- Most <b><span style="color:#d46644">upgrade</span></b> information can be found on the following page

	https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/

- It is not mandatory for all of the [[0 - Core Concepts Intro ✅|core control-plane components]] to have the 
- same <b><span style="color:#d46644">version</span></b>

![[clusteru-1.png]]

- Since the [[2 - Kube API server ✅|kube-apiserver]] is the most important component in the [[0 - Core Concepts Intro ✅|control-plane]] and is the component that all other components must talk to, none of the components can have a higher <b><span style="color:#d46644">version</span></b> than the [[2 - Kube API server ✅|kube-apiserver]]

- The [[3 - Kube Controller Manager ✅|controller-manager]] and [[4 - Kube Scheduler ✅|scheduler]] can be one <b><span style="color:#d46644">version</span></b> lower than the [[2 - Kube API server ✅|kube-apiserver]]

- The [[5 - Kubelet ✅|kubelet]] and [[6 - Kube Proxy ✅|kube-proxy]] can be two <b><span style="color:#d46644">versions</span></b> lower than the [[2 - Kube API server ✅|kube-apiserver]]

![[clusteru-2.png]]

- The <b><span style="color:#d46644">versioning</span></b> rules that apply to the [[0 - Core Concepts Intro ✅| control-plane components]] do not apply to <b><span style="color:#5c7e3e">kubectl</span></b>

- The <b><span style="color:#5c7e3e">kubectl</span></b> utility can be one <b><span style="color:#d46644">version</span></b> higher, the same, or one <b><span style="color:#d46644">version</span></b> lower than the [[2 - Kube API server ✅|kube-apiserver]]

![[clusteru-3.png]]

- This permissible skew in <b><span style="color:#d46644">versions</span></b> allow live <b><span style="color:#d46644">upgrades</span></b> to be carried out

### When should you <b><span style="color:#d46644">upgrade</span></b>

- At any time, <span style="color:#5c7e3e">Kubernetes</span> only supports up to three <b><span style="color:#d46644">minor versions</span></b>

- Be sure to <b><span style="color:#d46644">upgrade</span></b> your current <b><span style="color:#d46644">version</span></b> before the next <b><span style="color:#d46644">version</span></b> (which will kick your current <b><span style="color:#d46644">version</span></b> to unsupported status) is released

![[clusteru-4.png]]

- The recommended method of <b><span style="color:#d46644">upgrading</span></b> is to <b><span style="color:#d46644">upgrade</span></b> one <b><span style="color:#d46644">minor version</span></b> at a time
	- As opposed to just jumping to the latest <b><span style="color:#d46644">version</span></b>

- The <b><span style="color:#d46644">upgrade</span></b> process depends on how your [[0 - Core Concepts Intro ✅|cluster]] is setup
	- If the [[0 - Core Concepts Intro ✅|cluster]] is managed on a cloud platform, there are likely tools that make <b><span style="color:#d46644">upgrading</span></b> easy
	- If the [[0 - Core Concepts Intro ✅|cluster]] is managed with tools such as <b><span style="color:#5c7e3e">Kubeadm</span></b>, the tool will help you plan and <b><span style="color:#d46644">upgrade</span></b> the [[0 - Core Concepts Intro ✅|cluster]]

![[clusteru-5.png]]

- If the [[0 - Core Concepts Intro ✅|cluster]] was deployed from scratch, you must manually <b><span style="color:#d46644">upgrade</span></b> each component in the [[0 - Core Concepts Intro ✅|cluster]]

### Upgrading a <span style="color:#5c7e3e">Kubeadm</span> [[0 - Core Concepts Intro ✅|cluster]]

- Upgrading a [[0 - Core Concepts Intro ✅|cluster]] involves two major steps:
	- <b><span style="color:#d46644">upgrade</span></b> the [[0 - Core Concepts Intro ✅|master nodes]]
	- <b><span style="color:#d46644">upgrade</span></b> the [[0 - Core Concepts Intro ✅|worker nodes]]

- While the [[0 - Core Concepts Intro ✅|master node]] is being <b><span style="color:#d46644">upgraded</span></b>, the [[0 - Core Concepts Intro ✅|control-plane components]] such as the [[2 - Kube API server ✅|kube-apiserver]], [[4 - Kube Scheduler ✅|scheduler]], and [[3 - Kube Controller Manager ✅|controller managers]] go down briefly

- The [[0 - Core Concepts Intro ✅|master node]] going down doesn't mean your [[0 - Core Concepts Intro ✅|worker nodes]] and applications on the [[0 - Core Concepts Intro ✅|cluster]] are impacted

- Since the [[0 - Core Concepts Intro ✅|master node]] is down, all management functions are down
	- In this case, you cannot access the [[0 - Core Concepts Intro ✅|cluster]] using <span style="color:#5c7e3e">kubectl</span> or the <span style="color:#5c7e3e">Kubernetes</span> API
	- You cannot deploy new applications or modify existing ones
	- The [[3 - Kube Controller Manager ✅|controller managers]] won't function either
		- If a [[7 - Pods ✅|pod]] was to fail, a new [[7 - Pods ✅|pod]] will not be automatically brought back up

- Once the <b><span style="color:#d46644">upgrade</span></b> is complete and the [[0 - Core Concepts Intro ✅|cluster]] is back up, it should function as normal

- There are different strategies available to <b><span style="color:#d46644">upgrade</span></b> the [[0 - Core Concepts Intro ✅|worker nodes]]

- Strategy 1 to <b><span style="color:#d46644">upgrade</span></b> a [[0 - Core Concepts Intro ✅|worker node]] is to <b><span style="color:#d46644">upgrade</span></b> all of them at once
	- With this process, all [[7 - Pods ✅|pods]] are down and the application is unavailable to your users
	- Once the <b><span style="color:#d46644">upgrade</span></b> is complete, the [[0 - Core Concepts Intro ✅|nodes]] are back up, the new [[7 - Pods ✅|pods]] are scheduled, and users are able to access your application

- Strategy 2 to <b><span style="color:#d46644">upgrade</span></b> a [[0 - Core Concepts Intro ✅|worker node]] is to <b><span style="color:#d46644">upgrade</span></b> them one at a time
	- While <b><span style="color:#d46644">upgrading</span></b> one [[0 - Core Concepts Intro ✅|node]], the [[7 - Pods ✅|pods]] will be placed on other [[0 - Core Concepts Intro ✅|nodes]] while the current one is [[0 - Core Concepts Intro ✅|node]]  <b><span style="color:#d46644">upgraded</span></b>
	- Once the first [[0 - Core Concepts Intro ✅|node]] is back up, the second [[0 - Core Concepts Intro ✅|node]] goes down and those [[7 - Pods ✅|pods]] are then moved to the other [[0 - Core Concepts Intro ✅|nodes]] while that one is being <b><span style="color:#d46644">upgraded</span></b>
	- This process is repeated until all [[0 - Core Concepts Intro ✅|worker nodes]] are <b><span style="color:#d46644">upgraded</span></b>

- Strategy 3 to <b><span style="color:#d46644">upgrade</span></b> a [[0 - Core Concepts Intro ✅|worker node]] is to add new [[0 - Core Concepts Intro ✅|nodes]] (with newer <b><span style="color:#d46644">versions</span></b>) to the [[0 - Core Concepts Intro ✅|cluster]]
	- This method is convenient if you're on a cloud environment
	- Move the workload from the first [[0 - Core Concepts Intro ✅|node]] to the new [[0 - Core Concepts Intro ✅|node]] and terminate the original [[0 - Core Concepts Intro ✅|node]]
	- Continue this until you have all new <b><span style="color:#d46644">upgraded</span></b> [[0 - Core Concepts Intro ✅|nodes]] with the [[7 - Pods ✅|pods]] moved to them

- <span style="color:#5c7e3e">Kubeadm</span> has an <b><span style="color:#d46644">upgrade</span></b> command that helps in upgrading [[0 - Core Concepts Intro ✅|clusters]]
	- First run the <span style="color:red">kubeadm upgrade plan</span> command

	![[clusteru-6.png]]

	- This gives a bunch of information like the [[0 - Core Concepts Intro ✅|cluster]] <b><span style="color:#d46644">version</span></b> and <span style="color:#5c7e3e">Kubeadm</span> tool <b><span style="color:#d46644">version</span></b>
	- This also lists all of the [[0 - Core Concepts Intro ✅|control-plane components]], their current <b><span style="color:#d46644">versions</span></b>, and what <b><span style="color:#d46644">versions</span></b> they can be <b><span style="color:#d46644">upgraded</span></b> to
	- It also tells you what components need to be <b><span style="color:#d46644">upgraded</span></b> manually
	- Keep in mind that <span style="color:#5c7e3e">Kubeadm</span> does not install or <b><span style="color:#d46644">upgrade</span></b> [[5 - Kubelet ✅|kubelet]]
	- Finally, the command gives you the command to <b><span style="color:#d46644">upgrade</span></b> the [[0 - Core Concepts Intro ✅|cluster]]

- You must <b><span style="color:#d46644">upgrade</span></b> the <span style="color:#5c7e3e">Kubeadm</span> tool first before you can <b><span style="color:#d46644">upgrade</span></b> the [[0 - Core Concepts Intro ✅|cluster]]

- The <span style="color:#5c7e3e">Kubeadm</span> tool follows the same software <b><span style="color:#d46644">version</span></b> as <span style="color:#5c7e3e">Kubernetes</span>

- First <b><span style="color:#d46644">upgrade</span></b> the <span style="color:#5c7e3e">Kubeadm</span> tool with the OS's <b><span style="color:#d46644">upgrade</span></b> tool (ie. <span style="color:#5c7e3e">apt/yum</span> <b><span style="color:#d46644">upgrade</span></b>)
	- Then after  <b><span style="color:#d46644">upgrading</span></b> the [[0 - Core Concepts Intro ✅|cluster]], <b><span style="color:#d46644">upgrade</span></b> [[5 - Kubelet ✅|kubelet]] with the OS's <b><span style="color:#d46644">upgrade</span></b> tool

- Run the <span style="color:red">kubeadm upgrade apply</span> command with the <b><span style="color:#d46644">version</span></b>
	- Be sure to <b><span style="color:#d46644">upgrade</span></b> one <b><span style="color:#d46644">version</span></b> at a time

![[clusteru-7.png]]

- The <b><span style="color:#d46644">version</span></b> seen when running the <span style="color:red">kubectl get nodes</span> is showing the <b><span style="color:#d46644">version</span></b> of [[5 - Kubelet ✅|kubelet]] on each of the [[0 - Core Concepts Intro ✅|nodes]] registered with the [[2 - Kube API server ✅|kube-apiserver]] and not the <b><span style="color:#d46644">version</span></b> of the [[2 - Kube API server ✅|kube-apiserver]] itself
	- So when you first <b><span style="color:#d46644">upgrade</span></b> and then run the <span style="color:red">kubectl get nodes</span> command, you will still see an older [[5 - Kubelet ✅|kubelet]] <b><span style="color:#d46644">version</span></b>

	![[clusteru-8.png]]

	- Then <b><span style="color:#d46644">upgrade</span></b> the [[5 - Kubelet ✅|kubelet]] <b><span style="color:#d46644">version</span></b>

	![[clusteru-9.png]]

- Keep in mind that the <b><span style="color:#d46644">drain</span></b>, <b><span style="color:#d46644">cordon</span></b>, and <b><span style="color:#d46644">uncordon</span></b> commands are only to be run in the [[0 - Core Concepts Intro ✅|control-plane node]] (passing in any [[0 - Core Concepts Intro ✅|node]] name - master) since it is a <span style="color:#5c7e3e">kubectl</span> command
	- If it is run on a [[0 - Core Concepts Intro ✅|worker node]], you will receive an error

### Practice Problems

- What is the current version of the cluster

		kubectl get nodes

- You are tasked to upgrade the cluster. User's accessing the applications must not be impacted. And you cannot provision new VMs.

What strategy would you use to upgrade the cluster

You drain one node, allow the pods to go to the other, and upgrade the current node

- What is the latest stable version available for upgrade?

		kubeadm upgrade plan

- Drain the master node and mark it as unschedulable

		kubectl drain controlplane --ignore-daemonsets

- Upgrade the controlplane components to exact version v1.20.0

	1. `apt update && \`
	    `apt install -y --allow-change-held-packages kubeadm=1.20.0`

	1. `kubeadm upgrade plan`

	1. `kubeadm upgrade apply v1.20.0`

	1. `apt update && \``

apt install -y --allow-change-held-packages kubelet=1.20.0 kubectl=1.20.0

	1. `systemctl daemon-reload`
	    `systemctl restart kubelet`

	1. `kubectl get nodes`     # checking status

- Mark the controlplane node as schedulable again

		kubectl uncordon controlplane

- Now do the same and upgrade the worker node "node01" to version 1.20.0

(from control plane)

		kubectl drain node01 --ignore-daemonsets

ssh node01

	1. apt update && \
		  apt install -y --allow-change-held-packages kubeadm=1.20.0

	1. kubeadm upgrade plan

	1. kubeadm upgrade node

	1. apt update && \

apt install -y --allow-change-held-packages kubelet=1.20.0 kubectl=1.20.0

	1. systemctl daemon-reload && \  
    systemctl restart kubelet

	1. kubectl get nodes     # checking status

- Remove the restriction and mark node01 as schedulable again

	`kubectl uncordon node01`