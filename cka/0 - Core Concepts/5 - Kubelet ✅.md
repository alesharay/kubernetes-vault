- The <b><span style="color:#d46644">kubelet</span></b> **service**, in the <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|master/worker node]], registers the [[0 - Core Concepts Intro ✅|node]] with the <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]]

- The <b><span style="color:#d46644">kubelet</span></b> acts as a <i><span style="color:#477bbe">client</span></i>, and the <i><span style="color:#477bbe">runtime engine</span></i> acts as the <i><span style="color:#477bbe">server</span></i>.

- When <b><span style="color:#d46644">kubelet</span></b> receives instructions to load a [[7 - Pods ✅|pod]] onto a [[0 - Core Concepts Intro ✅|node]], it requests the <i><span style="color:#477bbe">container runtime engine</span></i> to pull the required [[11 - Image Security ✅|image]] and run an instance

- The <b><span style="color:#d46644">kubelet</span></b> then continues to monitor the state of the [[7 - Pods ✅|pod]] and the <i><span style="color:#477bbe">container(s)</span></i> in it and reports to the [[2 - Kube API server ✅|kube-apiserver]] on a timely basis

- If using the <b><i><span style="color:#5c7e3e">kubeadm</span></i></b> tool on your [[0 - Core Concepts Intro ✅|cluster]], it does not automatically install <b><span style="color:#d46644">kubelet</span></b>.
	- You must <u><b>always</b></u> manually install <b><span style="color:#d46644">kubelet</span></b> on your [[0 - Core Concepts Intro ✅|worker nodes]]
		- Do this by downloading <b><span style="color:#d46644">kubelet</span></b> from the <span style="color:#5c7e3e">Kubernetes</span> release page, extract it, and run it as a **service**