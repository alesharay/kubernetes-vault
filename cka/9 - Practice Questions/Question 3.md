### Question

[see answer](#answer)

Question 3

SIMULATION -

```bash
[student@node-1] $ kubectl config use-context mk8s
```

Task -

Given an existing Kubernetes cluster running `version 1.26.3`, upgrade all of the Kubernetes control plane and node components on the master node only to `version 1.26.4`.

Be sure to drain the master node before upgrading it (if necessary) and uncordon it after the upgrade (if necessary).

You can ssh to master node using:

```bash
[student@node-1] $ docker exec -it kind-mk8s sh
```

You can assume elevated privileges on the master node with the following command

```bash
[student@node-1] $ sudo -i
```

You are also expected to upgrade kubelet and kubectl on the master node.

Do not upgrade the worker nodes, etcd, the container manager, the CNI plugin, the DNS service or any other addons
























### Answer

[see question](#question)

![[q3a-1.jpg]]

![[q3a-2.jpg]]
