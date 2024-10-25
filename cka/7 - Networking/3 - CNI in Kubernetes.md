#flashcards/kubernetes/cka/networking

- As per <b><i><span style="color:#d46644">CNI</span></i></b>, [[7 - Pods ✅|container runtimes]], in the case <span style="color:#5c7e3e">Kubernetes</span>, is responsible for creating [[7 - Pods ✅|container]] [[0.4 - Network Namespaces ✅|network namespaces]] identifying and attaching those [[0.4 - Network Namespaces ✅|namespaces]] to the right <b><i><span style="color:#d46644">network</span></i></b> by calling the right <b><i><span style="color:#d46644">network plugins</span></i></b>

- In order for <span style="color:#5c7e3e">Kubernetes</span> to use <b><i><span style="color:#d46644">CNI plugins</span></i></b>, the <b><i><span style="color:#d46644">CNI plugin</span></i></b> must be invoked by the component within <span style="color:#5c7e3e">Kubernetes</span> that is responsible for creating [[7 - Pods ✅|containers]]
	- This component must then invoke the appropriate <b><i><span style="color:#d46644">network plugins</span></i></b> after the [[7 - Pods ✅|container]] is created

- The <b><i><span style="color:#d46644">CNI plugin</span></i></b> is configured in the [[5 - Kubelet ✅|kubelet]] service on each [[0 - Core Concepts Intro ✅|node]] in the [[0 - Core Concepts Intro ✅|cluster]]

![[cnik-1.png]]

- You can see the same information on viewing the running [[5 - Kubelet ✅|kubelet]] service

![[cnik-2.png]]

- The <b><i><span style="color:#d46644">CNI</span></i></b> bin directory has all the supported <b><i><span style="color:#d46644">CNI plugins</span></i></b> as executables
	- Such as [[0.1 - Switching, Routing, Gateways CNI in Kubernetes ✅|bridge]], <span style="color:#5c7e3e">DHCP</span>, <span style="color:#5c7e3e">flannel</span>
		- This directory is <span style="color:red">/opt/cni/bin</span> by default

![[cnik-3.png]]

- The <b><i><span style="color:#d46644">CNI</span></i></b> config directory has a set of configuration files, which is where [[5 - Kubelet ✅|kubelet]] looks to find out which plugin needs to be used

![[cnik-4.png]]

   - If there are multiple config files available, choose the one in alphabetical order

- The [[0.1 - Switching, Routing, Gateways CNI in Kubernetes ✅|bridge]] config file uses a format defined by the <b><i><span style="color:#d46644">CNI</span></i></b> standard for a plugin configuration file

![[cnik-5.png]]

- The [[5 - IPAM Weave|IPAM]] type <i><span style="color:#477bbe">host-local</span></i> means that the <span style="color:#5c7e3e">IP addresses</span> are managed locally on this <i><span style="color:#477bbe">host</span></i>, unlike a <span style="color:#5c7e3e">DHCP server</span> maintaining it remotely