#flashcards/kubernetes/cka/scheduling

- What if, when the [[4 - Kube Scheduler ✅|scheduler]] is <span style="color:#d46644">scheduling</span> [[7 - Pods ✅|pods]] to [[0 - Core Concepts Intro ✅|nodes]], none of the [[2 - Taints & Tolerations ✅|taints and tolerations]] or [[4 - Node Affinity ✅|nodeAffinity]] & [[3 - Node Selectors ✅|nodeSelector]] rules satisfies the needs
	- What if you want to use your own <span style="color:#d46644">scheduling</span> algorithm to place [[7 - Pods ✅|pods]] on [[0 - Core Concepts Intro ✅|nodes]]

- You can write your own <span style="color:#5c7e3e">Kubernetes</span> [[4 - Kube Scheduler ✅|scheduler]] program, package it and deploy it as the default [[4 - Kube Scheduler ✅|scheduler]] or as an additional [[4 - Kube Scheduler ✅|scheduler]] in the [[0 - Core Concepts Intro ✅|cluster]]

- Your <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]] can have <b><span style="color:#d46644">multiple schedulers</span></b> at the same time

- When creating [[7 - Pods ✅|pods]] or [[9 - Deployments ✅|deployments]], you can instruct <span style="color:#5c7e3e">Kubernetes</span> to have the [[7 - Pods ✅|pods]] <span style="color:#d46644">scheduled</span> by a specific [[4 - Kube Scheduler ✅|scheduler]]

- When deploying a [[4 - Kube Scheduler ✅|scheduler]] manually, by downloading the [[4 - Kube Scheduler ✅|scheduler]] binary and running it as a service (OS service - not <span style="color:#5c7e3e">Kubernetes</span> service), one of the options is the <span style="color:red">--scheduler-name</span> option
	- If this is not set, it will use the default [[4 - Kube Scheduler ✅|scheduler]]

- To deploy an additional [[4 - Kube Scheduler ✅|scheduler]], you can use the same [[4 - Kube Scheduler ✅|kube-scheduler]] binary
	- or use one that you may have built yourself (this would make the most sense)

![[mult-schedulers-1.png]]

NOTE: be sure to note the name you give the [[4 - Kube Scheduler ✅|scheduler]] as you will be using it in the [[7 - Pods ✅|pod]] or [[9 - Deployments ✅|deployment]] definition files later

- The <b><i><span style="color:#5c7e3e">kubeadm</span></i></b> tool deploys the [[4 - Kube Scheduler ✅|scheduler]] as a [[7 - Pods ✅|pod]]
	- The command property shows the associated options to start the [[4 - Kube Scheduler ✅|scheduler]]

![[mult-schedulers-2.png]]

* To make an additional [[4 - Kube Scheduler ✅|scheduler]], you can copy the <i><span style="color:#5c7e3e">manifest</span></i> file and change the name of the [[4 - Kube Scheduler ✅|scheduler]]

![[mult-schedulers-3.png]]

- An important option to focus on is the <span style="color:red">--leader-elect</span> option.
	- This option is used when you have multiple copies of the [[4 - Kube Scheduler ✅|scheduler]] running on different [[0 - Core Concepts Intro ✅|master nodes]] (this is a high availability setup where there are multiple [[0 - Core Concepts Intro ✅|master nodes]])
	- If multiple copies of the same [[4 - Kube Scheduler ✅|scheduler]] are running on the different [[0 - Core Concepts Intro ✅|nodes]], only one can be active at a time

- This is where the <span style="color:red">--leader-elect</span> option helps in choosing a [[4 - Kube Scheduler ✅|scheduler]] who will lead <span style="color:#d46644">scheduling</span> activities

- If you don't have multiple [[0 - Core Concepts Intro ✅|master nodes]], the <span style="color:red">--leader-elect</span> options should be "false".

- If you do have multiple [[0 - Core Concepts Intro ✅|master nodes]], you can pass in an additional parameter to set a "lock object" name (DEPRECATED: Will be removed in favor of leader-elect-resource-name)
	- This is to differentiate the new custom [[4 - Kube Scheduler ✅|scheduler]] from the default during the leader election process

![[mult-schedulers-4.png]]

- Create the [[7 - Pods ✅|pod]] using the <span style="color:red">kubectl create</span> command and run the <span style="color:red">kubectl get pods</span> command in the **kube-system** [[11 - Namespaces ✅|namespace]] to see the [[4 - Kube Scheduler ✅|schedulers]] running

![[mult-schedulers-5.png]]

- The next step is to configure a new [[7 - Pods ✅|pod]] or [[9 - Deployments ✅|deployment]] to use the new [[4 - Kube Scheduler ✅|scheduler]]

- In the definition file, add a field called <span style="color:#5c7e3e">schedulerName</span> and specify the name of the new [[4 - Kube Scheduler ✅|scheduler]]

![[mult-schedulers-6.png]]

- By default, if this property is not defined, it automatically uses the default [[4 - Kube Scheduler ✅|scheduler]]

- To know which [[4 - Kube Scheduler ✅|scheduler]] picked up the [[7 - Pods ✅|pod]] creation responsibilities, view the events using the <span style="color:red">kubectl get events</span> command
	- This lists all of the events in the current [[11 - Namespaces ✅|namespace]]

![[mult-schedulers-7.png]]

- You can also view the logs of the [[4 - Kube Scheduler ✅|scheduler]] using the <span style="color:red">kubectl logs</span> command

![[mult-schedulers-8.png]]

#### Practice Problems

- What is the name of the [[7 - Pods ✅|pod]] that deploys the default <span style="color:#5c7e3e">Kubernetes</span> [[4 - Kube Scheduler ✅|scheduler]] in this environment?

        kubectl get pods -A