#flashcards/kubernetes/cka/security

- <b><i><span style="color:#d46644">Root Certificates</span></i></b> are the <b><i><span style="color:#d46644">CA's pubilc</span></i></b> and <b><i><span style="color:#d46644">private keys</span></i></b> that are configured on <i><span style="color:#477bbe">CA servers</span></i> used to sign <b><i><span style="color:#d46644">server certificates</span></i></b>

- <b><i><span style="color:#d46644">Server Certificates</span></i></b> are <b><i><span style="color:#d46644">public</span></i></b> and <b><i><span style="color:#d46644">private key</span></i></b> pairs used to <i><span style="color:#477bbe">secure</span></i> connectivity
	- This is any <i><span style="color:#477bbe">server</span></i> that needs to be accessed by a <i><span style="color:#477bbe">client</span></i> to perform a task

- <b><i><span style="color:#d46644">Client Certificates</span></i></b> are configured on <i><span style="color:#477bbe">client servers</span></i> and requested by the <i><span style="color:#477bbe">servers</span></i> to verify the <i><span style="color:#477bbe">clients</span></i> trying to access them

- Use <i><span style="color:#477bbe">server</span></i>, <i><span style="color:#477bbe">root</span></i>, or <i><span style="color:#477bbe">client</span></i> as a naming convention for the [[3.1 - Certification Creation|certificates]] for simplicity

![[tlsk-1.png]]

- All communication between [[0 - Core Concepts Intro ✅|nodes]] in a [[0 - Core Concepts Intro ✅|cluster]] need to be encrypted
	- All interactions between all services and their <i><span style="color:#477bbe">clients</span></i> need to be <i><span style="color:#477bbe">secure</span></i>

- The two primary requirements for a <i><span style="color:#477bbe">server</span></i> to be <i><span style="color:#477bbe">secure</span></i> are to have
	- All the various services within the [[0 - Core Concepts Intro ✅|cluster]] to have <b><i><span style="color:#d46644">server certificates</span></i></b>
	- All the <i><span style="color:#477bbe">clients</span></i> to use <b><i><span style="color:#d46644">client certificates</span></i></b> to verify they are who they say they are

![[tlsk-2.png]]

##### Which <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]] components are <i><span style="color:#477bbe">servers</span></i> and which are <i><span style="color:#477bbe">clients</span></i>

### Servers

- The [[2 - Kube API server ✅|kube-apiserver]] is the [[2 - Kube API server ✅|API server]] that exposes an HTTPS service that other <i><span style="color:#477bbe">servers</span></i> as well as external <i><span style="color:#477bbe">users</span></i> use to manage the <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]]
	- This is a <i><span style="color:#477bbe">server</span></i> which requires [[3.1 - Certification Creation|certificates]] to <i><span style="color:#477bbe">secure</span></i> all communication with its <i><span style="color:#477bbe">clients</span></i>
	- We name the generated [[3.1 - Certification Creation|certificate]] <span style="color:red">apiserver.crt</span> and <b><i><span style="color:#d46644">private key</span></i></b> <span style="color:red">apiserver.key</span>

![[tlsk-3.png]]

- The names could be different based on who setup the [[0 - Core Concepts Intro ✅|cluster]]

- The [[1 - ETCD ✅|etcd server]] stores all information about the [[0 - Core Concepts Intro ✅|cluster]]
	- We name the generated [[3.1 - Certification Creation|certificate]] <span style="color:red">etcdserver.crt</span> and <b><i><span style="color:#d46644">private key</span></i></b> <span style="color:red">etcdserver.key</span>

![[tlsk-4.png]]

- The [[5 - Kubelet ✅|kubelet]] services on the [[0 - Core Concepts Intro ✅|worker nodes]] are <i><span style="color:#477bbe">servers</span></i> that also expose an HTTPS API endpoint that the [[2 - Kube API server ✅|kube-apiserver]] talks to in order to interact with [[0 - Core Concepts Intro ✅|worker nodes]]
	- We name the generated [[3.1 - Certification Creation|certificate]] <span style="color:red">kubelet.crt</span> and <b><i><span style="color:#d46644">private key</span></i></b> <span style="color:red">kubelet.key</span>

![[tlsk-5.png]]

### Clients

- The <i><span style="color:#477bbe">client</span></i> that access the [[2 - Kube API server ✅|kube-apiserver]] is the <i><span style="color:#477bbe">user's</span></i> and <i><span style="color:#477bbe">administrator's</span></i> <span style="color:#5c7e3e">kubectl</span> or a <span style="color:#5c7e3e">ReST API</span>

- The <i><span style="color:#477bbe">admin user</span></i> requires a [[3.1 - Certification Creation|certificate]] to [[1 - Authentication ✅|authenticate]] to the [[2 - Kube API server ✅|kube-apiserver]]
	- We name the generated [[3.1 - Certification Creation|certificate]] <span style="color:red">admin.crt</span> and <b><i><span style="color:#d46644">private key</span></i></b> <span style="color:red">admin.key</span>

![[tlsk-6.png]]

- The [[4 - Kube Scheduler ✅|scheduler]] talks to the [[2 - Kube API server ✅|kube-apiserver]] to look for [[7 - Pods ✅|pods]] that require scheduling and then gets the [[7 - Pods ✅|pods]] scheduled on the correct [[0 - Core Concepts Intro ✅|worker nodes]]
	- As far as the [[2 - Kube API server ✅|kube-apiserver]] is concerned, the [[4 - Kube Scheduler ✅|scheduler]] is just another <i><span style="color:#477bbe">client</span></i> like the <i><span style="color:#477bbe">admin user</span></i>

- The [[4 - Kube Scheduler ✅|scheduler]] needs to validate its identity using a <b><i><span style="color:#d46644">client TLS certificate</span></i></b>
	- It needs its own pair of [[3.1 - Certification Creation|certificates]] and <b><i><span style="color:#d46644">keys</span></i></b>
	- We name the generated [[3.1 - Certification Creation|certificate]] <span style="color:red">scheduler.crt</span> and <b><i><span style="color:#d46644">private key</span></i></b> <span style="color:red">scheduler.key</span>

![[tlsk-7.png]]

- The [[3 - Kube Controller Manager ✅|kube-controller manager]] is a <i><span style="color:#477bbe">client</span></i> that accesses the [[2 - Kube API server ✅|kube-apiserver]] and requires a [[3.1 - Certification Creation|certificate]] to [[1 - Authentication ✅|authenticate]]
	- We name the generated [[3.1 - Certification Creation|certificate]] <span style="color:red">controller-manager.crt</span> and <b><i><span style="color:#d46644">private key</span></i></b> <span style="color:red">controller-manager.key</span>

![[tlsk-8.png]]

- The [[6 - Kube Proxy ✅|kube-proxy]] is a <i><span style="color:#477bbe">client</span></i> that accesses the [[2 - Kube API server ✅|kube-apiserver]] and requires a [[3.1 - Certification Creation|certificate]] to [[1 - Authentication ✅|authenticate]]
	- We name the generated [[3.1 - Certification Creation|certificate]] <span style="color:red">kube-proxy.crt</span> and <b><i><span style="color:#d46644">private key</span></i></b> <span style="color:red">kube-proxy.key</span>

![[tlsk-9.png]]

------------------------------------------------------------------------------

- The <i><span style="color:#477bbe">servers</span></i> communicate amongst each other as well

- The [[2 - Kube API server ✅|kube-apiserver]] is the only <i><span style="color:#477bbe">server</span></i> that talks to the [[1 - ETCD ✅|etcd server]]

- As far as the [[1 - ETCD ✅|etcd server]] is concerned, the [[2 - Kube API server ✅|kube-apiserver]] is just another <i><span style="color:#477bbe">client</span></i> and thus needs to [[1 - Authentication ✅|authenticate]] the [[2 - Kube API server ✅|kube-apiserver]]
	- The same <b><i><span style="color:#d46644">keys</span></i></b> from serving its own API service can be used (<span style="color:red">apiservice.crt</span> and <span style="color:red">apiservice.key</span>) or you can generate new ones specifically for this purpose

- The [[2 - Kube API server ✅|kube-apiserver]] talks to the [[5 - Kubelet ✅|kubelet server]] on each of the individual [[0 - Core Concepts Intro ✅|nodes]]
	- This is how it monitors the [[0 - Core Concepts Intro ✅|worker nodes]]
	- The same <b><i><span style="color:#d46644">keys</span></i></b> from serving its own API service can be used (<span style="color:red">apiservice.crt</span> and <span style="color:red">apiservice.key</span>) or you can generate new ones specifically for this purpose

![[tlsk-10.png]]

- To generate [[3.1 - Certification Creation|certificates]] for the <span style="color:#5c7e3e">Kubernetes</span> components, we need a <b><i><span style="color:#d46644">certificate authority</span></i></b> (<b><i><span style="color:#d46644">CA</span></i></b>) to sign all of these [[3.1 - Certification Creation|certificates]]

- <span style="color:#5c7e3e">Kubernetes</span> requires you to have at least one [[3.1 - Certification Creation|certificate]] authority for your [[0 - Core Concepts Intro ✅|cluster]]

- It is possible to have more than <u>one</u> <b><i><span style="color:#d46644">certificate authority</span></i></b> for your [[0 - Core Concepts Intro ✅|cluster]]
	- One for all of the components in the [[0 - Core Concepts Intro ✅|cluster]]
	- Another specifically for [[1 - ETCD ✅|etcd]]

- If [[1 - ETCD ✅|etcd]] has its own <b><i><span style="color:#d46644">CA</span></i></b> for the [[0 - Core Concepts Intro ✅|cluster]], then the <b><i><span style="color:#d46644">etcd server certificates</span></i></b> and the <b><i><span style="color:#d46644">etcd client certificates</span></i></b> will be all signed by the [[1 - ETCD ✅|etcd server]] <b><i><span style="color:#d46644">CA</span></i></b>

- As we know, the <b><i><span style="color:#d46644">CA</span></i></b> has its own [[3.1 - Certification Creation|certificate]] and <b><i><span style="color:#d46644">private key</span></i></b>
	- We name the generated [[3.1 - Certification Creation|certificate]] <span style="color:red">ca.crt</span> and <b><i><span style="color:#d46644">private key</span></i></b> <span style="color:red">ca.key</span>