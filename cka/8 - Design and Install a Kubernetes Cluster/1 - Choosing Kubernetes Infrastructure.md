-  On a Unix laptop or a local machine, you can get started with cluster installation by installing the binaries manually and setting up a local cluster
	- This, however is a very tedious process

- Using a solution that automates installing binaries will help in setting up a cluster in a matter of minutes

- On windows, you cannot setup Kubernetes natively as there are no Windows binaries
	- This means you must rely on a virtualization software (i.e. Hyper-V, VMWare Workstation or VirtualBox)

- There are also solutions available to run Kubernetes components as Docker containers on Windows VMs
	- But even then, the Docker images are Linux based and under the hood they run on a small Linux OS create by Hyper-V for running Linux Docker containers

Some Simple Solutions

- Minikube deploys on a cluster easily

- Kubeadm can be used to run a single or multi-node cluster really but the required host must be provisioned with the supported configuration manually

- The difference between Minikube or Kind and Kubeadm is that Minikube or Kind provisions a VM with supported configuration by itself, whereas Kubeadm expects the VMs provisioned already

- Another difference between Minikube or Kind and Kubeadm is that Kubeadm allows for provisioning multi-node clusters, whereas the former doesn't

Turnkey or Hosted Solutions (production grade)

- Turnkey solutions are where you provision the required VMs and use some kind of tools or scripts to configure a Kubernetes cluster on them
	- This means you are responsible for creating, patching, and otherwise those VMs manually
	- But cluster management and maintenance are made easy using these tools

- OpenShift is a popular turnkey solution that is provided by RedHat, which is an open source container application platform and it built on top of Kubernetes

- OpenShift provides a set of additional tools and a nice GUI to create and manage Kubernetes constructs and easily integrate with CI/CD pipelines

- Cloud Foundry Container Runtime is a turnkey solution that is also an open source project, which helps in deploying and managing highly available Kubernetes clusters using their open-source tool called BOSH

- VMWare Cloud PKS is a  turnkey solution that allows you to leverage your existing VMWare environment for Kubernetes

- Vagrant provides a set of useful scripts to deploy a Kubernetes cluster on different cloud service providers

- Hosted solutions are more like Kubernetes as a Service solutions, where the cluster along with the required VMs are deployed by the provider and Kubernetes is configured on them by the provider as well.

- Google Container Engine (GCE) is a very popular Kubernetes as a Service offering on Google Cloud Platform (GCP)

- OpenShift Online is an offering from RedHat where you can gain access to a fully functional Kubernetes cluster online

- Azure has the Azure Kubernetes Service (AKS)

- Elastic Container Service for Kubernetes (EKS) is Amazon's hosted Kubernetes offering

** These turnkey and hosted solutions are just a few of the many available for use **