- <span style="color:#5c7e3e">Kubernetes</span> does not come with a full featured built-in [[1 - Monitor Cluster Components|monitoring]] solution

- There are many <span style="color:#5c7e3e">Kubernetes</span> [[1 - Monitor Cluster Components|monitoring]] solutions available,
	- open-source:
		-  <b><span style="color:#d46644">Metrics-Server</span></b>, Prometheus, and Elastic Stack
	- Proprietary:
		- Datadog and Dynatrace

### Heapster vs Metrics-Server

- <span style="color:#5c7e3e">Heapster</span> was one of the original projects that enabled [[1 - Monitor Cluster Components|monitoring]] and analysis features for <span style="color:#5c7e3e">Kubernetes</span>
	- This is now deprecated

- <b><span style="color:#d46644">Metrics-Server</span></b> is a slimmed down version of <span style="color:#5c7e3e">Heapster</span>

- You can have <u>one</u> <b><span style="color:#d46644">Metrics-Server</span></b> per <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]]

- <b><span style="color:#d46644">Metrics-Server</span></b> retrieves information from each [[7 - Pods|pod]] and [[0 - Core Concepts Intro ✅|node]] in the <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]], aggregates them and stores them in memory

- <b><span style="color:#d46644">Metrics-Server</span></b> is only an in-memory [[1 - Monitor Cluster Components|monitoring]] solution and does not store the <b><span style="color:#d46644">metrics</span></b> externally
	- As a result, you cannot see historical performance data

### How are pod metrics generated

- <span style="color:#5c7e3e">Kubernetes</span> runs a [[0 - Core Concepts Intro ✅|kubelet]] agent on each [[0 - Core Concepts Intro ✅|node]], which is responsible for retrieving instructions from the [[0 - Core Concepts Intro ✅|kube-apiserver]] and running [[7 - Pods|pods]] on the [[0 - Core Concepts Intro ✅|nodes]]

- [[0 - Core Concepts Intro ✅|Kubelet]] contains a subcomponent known as <span style="color:#5c7e3e">cAdvisor</span> or <span style="color:#5c7e3e">Container Advisor</span>.

- <span style="color:#5c7e3e">cAdvisor</span> is responsible for receiving performance <b><span style="color:#d46644">metrics</span></b> from [[7 - Pods|pods]], and exposing them through the [[0 - Core Concepts Intro ✅|Kubelet]] API to make them available for the <b><span style="color:#d46644">Metrics-Server</span></b>.

- If using <span style="color:#5c7e3e">minikube</span>, run the following command

		minikube addons enable metrics-server

- For all other environments, [[9 - Deployments|deploy]] the <b><span style="color:#d46644">metrics-server</span></b> by cloning the <b><span style="color:#d46644">metrics-server</span></b> [[9 - Deployments|deployment]] files from the github repository

	`git clone` [https://github.com/kubernetes-incubator/metrics-server](https://github.com/kubernetes-incubator/metrics-server)

- Then [[9 - Deployments|deploy]] the required components

![[monitor-1.png]]

- <b><span style="color:#d46644">Metrics-Server</span></b> takes a bit of time to collect and process data.

- Once processed, [[0 - Core Concepts Intro ✅|cluster]] performance can be viewed by running the <span style="color:red">kubectl top</span> command

- The <span style="color:red">kubectl top node</span> command provides CPU and memory consumption of each of the [[0 - Core Concepts Intro ✅|nodes]]

- The <span style="color:red">kubectl top pod</span> command shows performance <b><span style="color:#d46644">metrics</span></b> of [[7 - Pods|pods]] in <span style="color:#5c7e3e">Kubernetes</span>

![[monitor-2.png]]

### Practice Problems  
 

- Download metrics-server from github

git clone [https://gitbub.com/kubernetes-incubator/metrics-server.git](https://gitbub.com/kubernetes-incubator/metrics-server.git)

- Deploy the metrics-server by creating all of the components downloaded from github

		kubectl create -f DEFINITION_FILE_DIR/.

- Display node metrics

		kubectl top nodes

- Display pod metrics

		kubectl top pods