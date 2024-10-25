#flashcards/kubernetes/cka/core-concepts

- The ==<b><span style="color:#d46644">scheduler</span></b>== is responsible for appointing [[7 - Pods ✅|pods]] to [[0 - Core Concepts Intro ✅|nodes]]

- The <b><span style="color:#d46644">scheduler</span></b> is only responsible for ==<span style="color:#5c7e3e">deciding</span>== which [[7 - Pods ✅|pod]] goes on which [[0 - Core Concepts Intro ✅|node]]
	- It does not place the [[7 - Pods ✅|pod]] on the [[0 - Core Concepts Intro ✅|node]]. This is the job of the [[5 - Kubelet ✅|kubelet]]

- The <b><span style="color:#d46644">scheduler</span></b> makes sure that the correct ==[[7 - Pods ✅|pod]]== ends up on the correct [[0 - Core Concepts Intro ✅|node]]

- The <b><span style="color:#d46644">scheduler</span></b> looks at each [[7 - Pods ✅|pod]] and tries to identify the ==best [[0 - Core Concepts Intro ✅|node]]== for it
	- It does this in two phases:
		1. **Filters** nodes
			1. The <b><span style="color:#d46644">scheduler</span></b> ==filters out== the [[0 - Core Concepts Intro ✅|nodes]] that don't meet the necessary requirements for the [[7 - Pods ✅|pods]]
				1. Ie. cpu and memory requirements, etc…
		2. **Ranks** nodes
			1. The <b><span style="color:#d46644">scheduler</span></b> uses a ==priority algorithm== to assign a score to the [[0 - Core Concepts Intro ✅|nodes]] on a scale of 0-10
				1. Ie. The <b><span style="color:#d46644">scheduler</span></b> calculates the number of resources that would be free after the [[7 - Pods ✅|pod]] is placed on the [[0 - Core Concepts Intro ✅|node]]; this is applied to the priority ranking and the one with the most resources remaining will win

- To install a <b><span style="color:#d46644">scheduler</span></b>, download the binary from the <span style="color:#5c7e3e">Kubernetes</span> release page, extract it, and run it as a **service**

- (if using <b><i><span style="color:#5c7e3e">kubeadm</span></i></b>) the <b><span style="color:#d46644">scheduler</span></b> is setup as a [[7 - Pods ✅|pod]] in the <span style="color:#5c7e3e">kube-system</span> [[11 - Namespaces ✅|namespace]] and can be viewed from the **manifests** directory

	`cat /etc/kubernetes/manifests/kube-scheduler.yaml`