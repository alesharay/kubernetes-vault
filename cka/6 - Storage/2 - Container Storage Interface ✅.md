#flashcards/kubernetes/cka/storage

- In the past, <span style="color:#5c7e3e">Kubernetes</span> used <span style="color:#5c7e3e">Docker</span> alone as the <b><i><span style="color:#d46644">container runtime engine</span></i></b>

- This means that all of the code to work with <span style="color:#5c7e3e">Docker</span> was embedded into the <span style="color:#5c7e3e">Kubernetes</span> source code

- <b><i><span style="color:#d46644">Container Runtime Interfaces</span></i></b> (<b><i><span style="color:#d46644">CRI</span></i></b>) came to be because with other <b><i><span style="color:#d46644">container runtime engines</span></i></b> being introduced (such as <span style="color:#5c7e3e">rkt</span> and <b><i><span style="color:#d46644">cri-o</span></i></b>), it was important to open up and extend support to work with different <b><i><span style="color:#d46644">container runtimes</span></i></b> and not be dependent on <span style="color:#5c7e3e">Kubernetes</span> source code

- <b><i><span style="color:#d46644">Container Runtime Interfaces</span></i></b> (<b><i><span style="color:#d46644">CRI</span></i></b>) is a standard that defines how an orchestration solution (like <span style="color:#5c7e3e">Kubernetes</span>) would communicate with <b><i><span style="color:#d46644">container runtimes</span></i></b> (like <span style="color:#5c7e3e">Docker</span>)

![[container-storage-1.png]]

- In the future, if any new <b><i><span style="color:#d46644">container runtime</span></i></b> is developed, they can simply follow the <b><i><span style="color:#d46644">CRI</span></i></b> standards

- To extend support for different **networking solutions**, the [[3 - CNI in Kubernetes|Container Networking Interface]] ([[3 - CNI in Kubernetes|CNI]]) was introduced

- The [[3 - CNI in Kubernetes|CNI]] allows any new **networking** vendors to simply develop a plugin based on the [[3 - CNI in Kubernetes|CNI]] standards and make their solution work with <span style="color:#5c7e3e">Kubernetes</span>

![[container-storage-2.png]]

- The <b><i><span style="color:#d46644">Container Storage Interface</span></i></b> (<b><i><span style="color:#d46644">CSI</span></i>)</b> supports multiple <b><i><span style="color:#d46644">storage</span></i></b> solutions

![[container-storage-3.png]]

- With <b><i><span style="color:#d46644">CSI</span></i></b>, you can now write your own drivers for your own <b><i><span style="color:#d46644">storage</span></i></b> to work with <span style="color:#5c7e3e">Kubernetes</span>

- <b><i><span style="color:#d46644">CSI</span></i></b> is not a <span style="color:#5c7e3e">Kubernetes</span> specific standard as it is meant to be a universal standard that if implemented, allows any <span style="color:#5c7e3e">container orchestration</span> tool to work with any <b><i><span style="color:#d46644">storage vendor</span></i></b> with a supported plugin

- Currently, <span style="color:#5c7e3e">Kubernetes</span>, <span style="color:#5c7e3e">CloudFoundry</span>, and <span style="color:#5c7e3e">Mesos</span> are on board with <b><i><span style="color:#d46644">CSI</span></i></b>

- The <b><i><span style="color:#d46644">CSI</span></i></b> defines a set of remote procedure calls (<span style="color:#5c7e3e">RPCs</span>) that will be called by the <span style="color:#5c7e3e">container orchestrator</span> and must be implemented by <b><i><span style="color:#d46644">storage drivers</span></i></b>

- Ex. <b><i><span style="color:#d46644">CSI</span></i></b> says that when a pod is created and requires a [[3 - Volumes ✅|volume]], the <span style="color:#5c7e3e">container orchestrator</span> should call the <span style="color:red">create volume</span> <span style="color:#5c7e3e">RPC</span> and pass a set of details such as the [[3 - Volumes ✅|volume]] name

- After calling the <span style="color:red">create volume</span> <span style="color:#5c7e3e">RPC</span>, the <b><i><span style="color:#d46644">storage driver</span></i></b> should implement this <span style="color:#5c7e3e">RPC</span>, handle the request, provision a new [[3 - Volumes ✅|volume]] on the <b><i><span style="color:#d46644">storage array</span></i></b> and return the results of the operation

- Similar to the <span style="color:red">create volume</span> <span style="color:#5c7e3e">RPC</span>, the <span style="color:#5c7e3e">container orchestrator</span> should call the <span style="color:red">delete volume</span> <span style="color:#5c7e3e">RPC</span> when a [[3 - Volumes ✅|volume]] is to be deleted and the <b><i><span style="color:#d46644">storage</span></i></b> driver should implement the code to decommission the [[3 - Volumes ✅|volume]] from the array when that call is made

- The [specification](https://github.com/container-storage-interface/spec) details exactly what parameters should be passed by the caller, what should be received by the solution and what error codes should be exchanged