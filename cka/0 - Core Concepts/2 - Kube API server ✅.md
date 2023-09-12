- The <b><span style="color:#d46644">kube-apiserver</span></b> is the primary management tool of Kubernetes

- When you run [[0 - Core Concepts Intro ✅|kubectl]], the [[0 - Core Concepts Intro ✅|kubectl]] utility reaches out to the <b><span style="color:#d46644">kube-apiserver</span></b>

- The <b><span style="color:#d46644">kube-apiserver</span></b> first authenticates the request and validates it.
	- It then retrieves the data from the [[1 - ETCD ✅|ETCD datastore]] and responds back with the requested information

- Technically, you don't need to use the [[0 - Core Concepts Intro ✅|kubectl]] utility, you could invoke the API directly

		curl -X POST /api/v1/namespaces/default/pods

- The [[4 - Kube Scheduler ✅|scheduler]] continuously monitors the <b><span style="color:#d46644">API server</span></b>

- The <b><span style="color:#d46644">kube-apiserver</span></b> is at the center of all the different tasks that need to be performed to make a change in the [[0 - Core Concepts Intro ✅|cluster]]

- To confirm, the <b><span style="color:#d46644">kube-apiserver</span></b>is responsible for:
	- Authenticating and validating requests
	- Storing and retrieving data in the [[1 - ETCD ✅|ETCD datastore]]
		- It's the only component that communicates directly with the [[1 - ETCD ✅|ETCD datastore]] 