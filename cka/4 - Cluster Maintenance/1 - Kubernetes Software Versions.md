- We know that when we install the Kubernetes cluster, we install a specific version of Kubernetes

- We can see the Kubernetes version by running the <span style="color:red">kubectl get nodes</span> command

- The Kubernetes release version consist of 3 parts

1. Major Version
2. Minor Version
3. Patch Version

- The minor versions are released every few months with new features and functionalities

- The patch versions are released more often with critical bug fixes

- Similar to most other applications, Kubernetes follows a standard software release versioning procedure

![[ksoftware-1.png]]

- The first major version of Kubernetes, 1.0, was released on July of 2015

- The latest stable version of Kubernetes is 1.26.0

- You will also see alpha and beta releases

- All of the bug fixes and improvements first go into an alpha release (tagged alpha in this release)

- From there, they make their way to beta release where the code is well tested
- And finally they make their way to the main stable release

- You can find all of the Kubernetes releases in the releases page of the Kubernetes Github repository

[https://kubernetes.io/releases/](https://kubernetes.io/releases/)

- Download the kubernetes.tar.gz file and extract it to find executables for all the Kubernetes components

- The package has all of the control plane components in it

![[ksoftware-2.png]]

- All of the components in the downloaded package have the same version

- kube-apiserver, controller-manager, kube-scheduler, kubelet, kube-proxy

- There are other components within the control plane that do not have the same version

- Etcd cluster, core DNS server