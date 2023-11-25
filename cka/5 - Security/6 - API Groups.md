[https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.26/#-strong-api-groups-strong](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.26/#-strong-api-groups-strong)-

- The <span style="color:#5c7e3e">Kubernetes</span> <b><i><span style="color:#d46644">API</span></i></b> is <b><i><span style="color:#d46644">grouped</span></i></b> into multiple <b><i><span style="color:#d46644">groups</span></i></b> based on their purpose
	- Those <b><i><span style="color:#d46644">groups</span></i></b> are the <i><span style="color:#477bbe">endpoints</span></i> for when the [[2 - Kube API server ✅|API server]] is contacted
		- ie. [https://kube-master:6443/version](https://kube-master:6443/version) = the <b><i><span style="color:#d46644">version group</span></i></b>
		- ie. [https://kube-master:6443/api/v1/pods](https://kube-master:6443/api/v1/pods) = the <b><i><span style="color:#d46644">API group</span></i></b>

- The common <b><i><span style="color:#d46644">API groups</span></i></b> include:

	- /metrics
	- /healthz
	- /version
	- /api
	- /apis
	- /logs

- The <b><i><span style="color:#d46644">/version API</span></i></b> is for viewing the version of the [[0 - Core Concepts Intro ✅|cluster]]

- The <b><i><span style="color:#d46644">/metrics</span></i></b> and <b><i><span style="color:#d46644">/health APIs</span></i></b> are used to monitor the health of the [[0 - Core Concepts Intro ✅|cluster]]

- The <b><i><span style="color:#d46644">/logs API</span></i></b> is used to integrate with third-party logging applications

- The <b><i><span style="color:#d46644">/api</span></i></b> and <b><i><span style="color:#d46644">/apis APIs</span></i></b> are responsible for [[0 - Core Concepts Intro ✅|cluster]] functionality

- The <b><i><span style="color:#d46644">/api API</span></i></b> is the <b><i><span style="color:#d46644">core group</span></i></b> and the <b><i><span style="color:#d46644">/apis API</span></i></b> is the <b><i><span style="color:#d46644">named group</span></i></b>

- The <b><i><span style="color:#d46644">core group</span></i></b> is where all core functionality exists

![[api-1.png]]

- The <b><i><span style="color:#d46644">named group APIs</span></i></b> are more organized versions and are where any new functionality will be placed

![[api-2.png]]

- With <b><i><span style="color:#d46644">named groups</span></i></b> (<b><i><span style="color:#d46644">/apis</span></i></b>), the direct following <i><span style="color:#477bbe">endpoint</span></i> is the <b><i><span style="color:#d46644">API groups</span></i></b> and the <i><span style="color:#477bbe">endpoints</span></i> following are the resources that have a list of actions for each

- The actions for an <b><i><span style="color:#d46644">API group</span></i></b> resource are known as <b><i><span style="color:#d46644">verbs</span></i></b>

- The <span style="color:#5c7e3e">Kubernetes</span> <b><i><span style="color:#d46644">API</span></i></b> reference page can indicate what the <b><i><span style="color:#d46644">API group</span></i></b> is for each object
	- When an object is selected, the first section in the documentation page shows its <b><i><span style="color:#d46644">group</span></i></b> details

![[api-3.png]]

- The <b><i><span style="color:#d46644">API groups</span></i></b> can also be viewed on the <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]] by navigating to port 6443 without a path

![[api-4.png]]

![[api-5.png]]

- If accessing the <b><i><span style="color:#d46644">API</span></i></b> through curl, you will not be allowed access except to certain <b><i><span style="color:#d46644">APIs</span></i></b> like version, as you have not specified any [[1 - Authentication ✅|authentication]] mechanisms

- To [[1 - Authentication ✅|authenticate]] to the <b><i><span style="color:#d46644">API</span></i></b> using the command line, pass the [[3.1 - Certification Creation|certificate]] file

![[api-6.png]]

- An alternative option to accessing the <span style="color:#5c7e3e">Kubernetes</span> <b><i><span style="color:#d46644">API</span></i></b> is to start a [[13 - Kubectl Apply ✅|kubectl]] proxy <i><span style="color:#477bbe">client</span></i>

- The [[13 - Kubectl Apply ✅|kubectl]] proxy command launches a proxy service locally on port 8001 and uses credentials and [[3.1 - Certification Creation|certificates]] from your [[5 - Kubeconfig ✅|kubeconfig]] file to [[1 - Authentication ✅|authenticate]] to the [[0 - Core Concepts Intro ✅|cluster]]

![[api-7.png]]

- [[6 - Kube Proxy ✅|kube-proxy]] and [[13 - Kubectl Apply ✅|kubectl]] proxy are not the same

- As a reminder, [[6 - Kube Proxy ✅|kube-proxy]] is used to enable connectivity between [[7 - Pods ✅|pods]] and [[10 - Services ✅|services]] across different [[0 - Core Concepts Intro ✅|nodes]] in a [[0 - Core Concepts Intro ✅|cluster]]

- [[13 - Kubectl Apply ✅|kubectl]] proxy is an HTTP proxy service created by the [[13 - Kubectl Apply ✅]] utility to access the [[2 - Kube API server ✅|kube-apiserver]]

### Summary

- All resources in <span style="color:#5c7e3e">Kubernetes</span> are <b><i><span style="color:#d46644">grouped</span></i></b> into different <b><i><span style="color:#d46644">API groups</span></i></b>

- At the top level, there are core and named <b><i><span style="color:#d46644">API groups</span></i></b>

- Under <b><i><span style="color:#d46644">named API groups</span></i></b> you have one <b><i><span style="color:#d46644">API group</span></i></b> for each section

- Under the lower-level <b><i><span style="color:#d46644">API groups</span></i></b>, you have the different resources which each has a set of associated actions known as <b><i><span style="color:#d46644">verbs</span></i></b>