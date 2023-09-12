- The <b><i><span style="color:#d46644">networking</span></i></b> at the [[7 - Pods ✅|pod]] layer is another crucial function of a <span style="color:#5c7e3e">Kubernetes</span> <b><i><span style="color:#d46644">network</span></i></b>

- <span style="color:#5c7e3e">Kubernetes</span> does not come with a built-in solution for <b><i><span style="color:#d46644">pod networking</span></i></b>

- This must be done manually

- <span style="color:#5c7e3e">Kubernetes</span> has laid out the requirements for <b><i><span style="color:#d46644">pod networking</span></i></b>
	- Every [[7 - Pods ✅|pod]] should have an <span style="color:#5c7e3e">IP address</span>
	- Every [[7 - Pods ✅|pod]] should be able to communicate with every other [[7 - Pods ✅|pod]] in the same [[0 - Core Concepts Intro ✅|node]] using that same <span style="color:#5c7e3e">IP address</span>
	- Every [[7 - Pods ✅|pod]] should be able to communicate with every other [[7 - Pods ✅|pod]] on other [[0 - Core Concepts Intro ✅|nodes]] without <span style="color:#5c7e3e">IP address</span> using that same <span style="color:#5c7e3e">IP address</span>

- There are many <b><i><span style="color:#d46644">networking</span></i></b> solutions out there that can implement these requirements

![[pod-networking-1.png]]

- We create a **script** for the <b><i><span style="color:#d46644">pod networking</span></i></b> and [[0.6 - CNI|CNI]] acts as a middleman to run that **script** automatically

- [[0.6 - CNI|CNI]] tells <span style="color:#5c7e3e">Kubernetes</span> that this is how you should call a script as soon as you create a [[7 - Pods ✅|container]], and [[0.6 - CNI|CNI]] tells us "this is how your **script** should look

- Whenever a [[7 - Pods ✅|container]] is created, the [[5 - Kubelet ✅|kubelet]] looks at the [[0.6 - CNI|CNI]] configuration passed as a command-line argument when it is run, and identifies the **script's** name
	- It then looks at the [[0.6 - CNI|CNI]] bin directory to find the **script**
	- It executes the script with the <span style="color:red">add</span> command and the name of the [[0.4 - Network Namespaces|namespace]] of the [[7 - Pods ✅|container]]
	- The **script** takes care of the rest