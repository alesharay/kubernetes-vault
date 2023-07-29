- As the admin of the cluster, when you need to add new admins to have access to the cluster, they need their own certificates and keys

- This new admin would create their own key-pair, generate a CSR, and send it to the current admin who has access to the CA server

- When the current admin receives the new admin's CSR, the current admin would sign the CSR using their CA server's private key and root certificate, then send back the signed certificate to the new admin

- The new admin is now authenticated to the cluster

- The CA is a pair of key and certificate files that have been generated

- Whoever gains access to the CA private key and certificate can sign any certificate in the Kubernetes environment and can create as many users as they want with whatever privileges they want

- Whatever secure server holds the CA private key and certificate is then the CA server

- Any time you want to sign a certificate with the CA private key and certificate file, the only way to do this is by logging into the CA server

- If the CA files are located on the master node, then the master node is also the CA server

- This is where the kubeadm tool places the CA files

- While CSRs can be signed manually, with larger teams and continuous growth, you need an easier way of managing certificates, carrying out the CSR signing process, and rotating certificates when they expire

- Kubernetes has a built-in certificate API that can manage, sign, and rotate certificates

- With the certificate API, you send the CSR directly to Kubernetes through an API call

- When a CSR is sent through an API call, rather than the admin manually logging into the CA server to sign, they create and Kubernetes API object called the CertificateSigningRequest object

- Once the CertificateSigningRequest object is created, all CSRs can be seen by the administrators of the cluster

- Once the CSR object is created, the requests can now easily be reviewed and approved using kubectl commands

- The certificate can then be extracted and shared with the new admin

------------------------------------------------------------------------------------------------------

### Process

1. A user creates a key-pair

![[certa-1.png]]

1. The user generates a CSR using the private key with their name on it, and sends the request to the admin

![[certa-2.png]]

1. The admin takes the CSR and creates a CertificateSigningRequest object with they base64 private key included

1. This is created like any other Kubernetes object, using a manifest file

![[certa-3.png]]

1. The spec section of the CertificateSigningRequest object contains:

1. groups

1. Groups the user should be part of

3. usages

1. Usages of the account (as a list of strings)

5. Request

1. Where the CSR sent by the user is specified (base64 encoded)

![[certa-4.png]]

1. All CSR objects can be seen by administrators by running the `kubectl get csr` command

![[certa-5.png]]

1. Identify and review the specified request. Then approve the request using the `kubectl certificate approve` command with the name of the CSR

![[certa-6.png]]

1. Kubernetes signs the certificate, then generates a certificate key-pair for the user

1. This certificate can then be extracted and shared with the user

1. To view the certificate, use the `kubectl get csr` command with the `-o yaml` option

![[certa-7.png]]

1. The generated certificate is part of the output (as a base64 encoded string)

------------------------------------------------------------------------------------------------------

- All certificate operations are carried out by the kube-controller-manager

- The kube-controller-manager has controllers called CSR-Approving, CSR-Signing, etc… that are responsible for carrying out the CSR tasks

- The kube-controller-manager has two options where you can specify the CA certificate and private key

![[certa-8.png]]

### Practice Problems

- A new member (Andrew) has joined the team and requires access to the cluster. His CSR is at /root. Inspect it

		openssl req -in /root/andrew.csr -text -noout

- Create a CSR object with the name andrew with the contents of the andrew.csr file

	(*As of Kubernetes 1.19, the apiVersion to use for CSR objects is certificates.k8s.io/v1)

	(**An additional filed called signerName should be added when creating the CSR. Use the built-in signer kubernetes.io/kube-apiserver.client)

![[certa-9.png]]

- What is the condition of the newly created CSR object?

		kubectl get csr

(jot down status of the andrew csr object)

Pending

- Approve the newly created CSR object

		kubectl certificate approve andrew

- During a routine check you realized that there is a new CSR request in place. What is the name of this request?

		kubectl get csr -A

	(jot down pending request name)

- Hmmm… You are not aware of an incoming request. What groups is the CSR requesting access to?

		kubectl get csr <csr name> -o yaml

	(jot down the groups)

- This doesn't look right, reject that request

		kubectl certificate deny <csr name>

- Lets get rid of the denied request. Delete the CSR object

		kubectl delete csr <csr name>