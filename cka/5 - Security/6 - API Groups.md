[https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.26/#-strong-api-groups-strong](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.26/#-strong-api-groups-strong)-

- The Kubernetes API is grouped into multiple groups based on their purpose

- Those groups are the endpoints for when the API server is contacted

- ie. [https://kube-master:6443/version](https://kube-master:6443/version) = the version group
- ie. [https://kube-master:6443/api/v1/pods](https://kube-master:6443/api/v1/pods) = the api group

- The common API groups include:

- /metrics
- /healthz
- /version
- /api
- /apis
- /logs

- The /version API is for viewing the version of the cluster

- The /metrics and /healthz APIs are used to monitor the health of the cluster

- The /logs API is used to integrate with third-party logging applications

- The /api and /apis APIs are responsible for cluster functionality

- The /api API is the core group and the /apis API is the named group

- The core group is where all core functionality exists

![[api-1.png]]

- The named group APIs are more organized versions and are where any new functionality will be placed

![[api-2.png]]

- With named groups (/apis), the direct following endpoint is the API groups and the endpoints following are the resources that have a list of actions for each

- The actions for an API group resource are known as verbs

- The Kubernetes API reference page can indicate what the API group is for each object

- When an object is selected, the first section in the documentation page shows its group details

![[api-3.png]]

- The API groups can also be viewed on the Kubernetes cluster by navigating to port 6443 without a path

![[api-4.png]]

![[api-5.png]]

- If accessing the API through curl, you will not be allowed access except to certain APIs like version, as you have not specified any authentication mechanisms

- To authenticate to the API using the command line, pass the certificate file

![[api-6.png]]

- An alternative option to accessing the Kubernetes API is to start a kubectl proxy client

- The <span style="color:red">kubectl proxy</span> command launches a proxy service locally on port 8001 and uses credentials and certificates from your kubeconfig file to authenticate to the cluster

![[api-7.png]]

- Kube-proxy and kubectl proxy are not the same

- As a reminder, kube-proxy is used to enable connectivity between pods and services across different nodes in a cluster

- kubectl proxy is an HTTP proxy service created by the kubectl utility to access the kube-apiserver

### Summary

- All resources in Kubernetes are grouped into different API groups

- At the top level, there are core and named API groups

- Under named API groups you have one API group for each section

- Under the lower-level API groups, you have the different resources which each has a set of associated actions known as verbs