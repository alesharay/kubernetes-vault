- As the <i><span style="color:#477bbe">admin</span></i> of the [[0 - Core Concepts Intro ✅|cluster]], when you need to add new <i><span style="color:#477bbe">admins</span></i> to have access to the [[0 - Core Concepts Intro ✅|cluster]], they need their own [[3.1 - Certification Creation|certificate]] and [[2 - TLS Basics ✅|keys]]

- This new <i><span style="color:#477bbe">admin</span></i> would create their own [[2 - TLS Basics ✅|key-pair]], generate a [[3.1 - Certification Creation|CSR]], and send it to the current <i><span style="color:#477bbe">admin</span></i> who has access to the <i><span style="color:#477bbe">CA server</span></i>

- When the current <i><span style="color:#477bbe">admin</span></i> receives the new <i><span style="color:#477bbe">admin's</span></i> [[3.1 - Certification Creation|CSR]], the current <i><span style="color:#477bbe">admin</span></i> would [[2 - TLS Basics ✅|sign]] the [[3.1 - Certification Creation|CSR]] using their <i><span style="color:#477bbe">CA server's</span></i> [[2 - TLS Basics ✅|private key]] and [[3.1 - Certification Creation|root certificate]], then send back the [[3.1 - Certification Creation|signed certificate]] to the new <i><span style="color:#477bbe">admin</span></i>
	- The new <i><span style="color:#477bbe">admin</span></i> is now [[1 - Authentication ✅ ✅|authenticated]] to the [[0 - Core Concepts Intro ✅|cluster]]

- The [[2 - TLS Basics ✅|CA]] is a pair of [[2 - TLS Basics ✅|key]] and [[3.1 - Certification Creation|certificate]] files that have been generated

- Whoever gains access to the [[2 - TLS Basics ✅|CA private key]] and [[3.1 - Certification Creation|certificate]] can [[2 - TLS Basics ✅|sign]] any [[3.1 - Certification Creation|certificate]] in the <span style="color:#5c7e3e">Kubernetes</span> environment and can create as many <i><span style="color:#477bbe">users</span></i> as they want with whatever privileges they want

- Whatever secure <i><span style="color:#477bbe">server</span></i> holds the [[2 - TLS Basics ✅|CA private key]] and [[3.1 - Certification Creation|certificate]] is then the <i><span style="color:#477bbe">CA server</span></i>

- Any time you want to [[2 - TLS Basics ✅|sign]] a [[3.1 - Certification Creation|certificate]] with the [[2 - TLS Basics ✅|CA private key]] and [[3.1 - Certification Creation|certificate]] file, the only way to do this is by logging into the <i><span style="color:#477bbe">CA server</span></i>

- If the [[2 - TLS Basics ✅|CA]] files are located on the [[0 - Core Concepts Intro ✅|master node]], then the [[0 - Core Concepts Intro ✅|master node]] is also the <i><span style="color:#477bbe">CA server</span></i>
	- This is where the <span style="color:#5c7e3e">kubeadm</span> tool places the [[2 - TLS Basics ✅|CA]] files

- While [[3.1 - Certification Creation|CSR]] can be [[2 - TLS Basics ✅|signed]] manually, with larger teams and continuous growth, you need an easier way of managing [[3.1 - Certification Creation|certificates]], carrying out the [[3.1 - Certification Creation|CSR]] [[2 - TLS Basics ✅|signing]] process, and rotating [[3.1 - Certification Creation|certificates]] when they expire

- <span style="color:#5c7e3e">Kubernetes</span> has a built-in <b><i><span style="color:#d46644">certificate API</span></i></b> that can manage, [[2 - TLS Basics ✅|sign]], and rotate [[3.1 - Certification Creation|certificates]]

- With the <b><i><span style="color:#d46644">certificate API</span></i></b>, you send the [[3.1 - Certification Creation|CSR]] directly to <span style="color:#5c7e3e">Kubernetes</span> through an <b><i><span style="color:#d46644">API</span></i></b> call

- When a [[3.1 - Certification Creation|CSR]] is sent through an <b><i><span style="color:#d46644">API</span></i></b> call, rather than the <i><span style="color:#477bbe">admin</span></i> manually logging into the <i><span style="color:#477bbe">CA server</span></i> to [[2 - TLS Basics ✅|sign]], they create and <span style="color:#5c7e3e">Kubernetes</span> API object called the [[3.1 - Certification Creation|CertificateSigningRequest]] object

- Once the [[3.1 - Certification Creation|CertificateSigningRequest]] object is created, all [[3.1 - Certification Creation|CSRs]] can be seen by the <i><span style="color:#477bbe">administrators</span></i> of the [[0 - Core Concepts Intro ✅|cluster]]

- Once the [[3.1 - Certification Creation|CSR]] object is created, the requests can now easily be reviewed and approved using <span style="color:#5c7e3e">kubectl</span> commands
	- The [[3.1 - Certification Creation|certificate]] can then be extracted and shared with the new <i><span style="color:#477bbe">admin</span></i>

------------------------------------------------------------------------------------------------------

### Process

1. A <i><span style="color:#477bbe">user</span></i> creates a [[2 - TLS Basics ✅|key-pair]]

		![[certa-1.png]]

2. The <i><span style="color:#477bbe">user</span></i> generates a [[3.1 - Certification Creation|CSR]] using the [[2 - TLS Basics ✅|private key]] with their name on it, and sends the request to the <i><span style="color:#477bbe">admin</span></i>

		![[certa-2.png]]

3. The <i><span style="color:#477bbe">admin</span></i> takes the [[3.1 - Certification Creation|CSR]] and creates a [[3.1 - Certification Creation|CertificateSigningRequest]] object with they <span style="color:#5c7e3e">base64</span> [[2 - TLS Basics ✅|private key]] included
	1. This is created like any other <span style="color:#5c7e3e">Kubernetes</span> object, using a manifest file

		![[certa-3.png]]

	2. The spec section of the [[3.1 - Certification Creation|CertificateSigningRequest]] object contains:
		1. <i><span style="color:#477bbe">groups</span></i>
			1. <i><span style="color:#477bbe">Groups</span></i> the <i><span style="color:#477bbe">user</span></i> should be part of
		2. usages
			1. Usages of the account (as a list of strings)
		3. Request
			1. Where the [[3.1 - Certification Creation|CSR]] sent by the <i><span style="color:#477bbe">user</span></i> is specified (<span style="color:#5c7e3e">base64</span> encoded)

		![[certa-4.png]]

	4. All [[3.1 - Certification Creation|CSR]] objects can be seen by <i><span style="color:#477bbe">administrators</span></i> by running the <span style="color:red">kubectl get csr</span> command

		![[certa-5.png]]

	5. Identify and review the specified request. Then approve the request using the <span style="color:red">kubectl [[3.1 - Certification Creation|certificate]] approve</span> command with the name of the [[3.1 - Certification Creation|CSR]]

		![[certa-6.png]]

	6. <span style="color:#5c7e3e">Kubernetes</span> [[2 - TLS Basics ✅|signs]] the [[3.1 - Certification Creation|certificate]], then generates a [[3.1 - Certification Creation|certificate]] [[2 - TLS Basics ✅|key-pair]] for the <i><span style="color:#477bbe">user</span></i>

	7. This [[3.1 - Certification Creation|certificate]] can then be extracted and shared with the <i><span style="color:#477bbe">user</span></i>

	8. To view the [[3.1 - Certification Creation|certificate]], use the <span style="color:red">kubectl get csr</span> command with the <span style="color:red">-o yaml</span> option

		![[certa-7.png]]

	9. The generated [[3.1 - Certification Creation|certificate]] is part of the output (as a <span style="color:#5c7e3e">base64</span> encoded string)

------------------------------------------------------------------------------------------------------

- All [[3.1 - Certification Creation|certificate]] operations are carried out by the [[3 - Kube Controller Manager ✅|kube-controller-manager]]

- The [[3 - Kube Controller Manager ✅|kube-controller-manager]] has [[3 - Kube Controller Manager ✅|controllers]] called [[3.1 - Certification Creation|CSR]]-Approving, [[3.1 - Certification Creation|CSR]]-[[2 - TLS Basics ✅|signing]], etc… that are responsible for carrying out the [[3.1 - Certification Creation|CSR]] tasks

- The [[3 - Kube Controller Manager ✅|kube-controller-manager]] has two options where you can specify the [[3.1 - Certification Creation|CA certificate]] and [[2 - TLS Basics ✅|private key]]

![[certa-8.png]]

### Practice Problems

- A new member (Andrew) has joined the team and requires access to the cluster. His CSR is at /root. Inspect it

		openssl req -in /root/andrew.csr -text -noout

- Create a CSR object with the name andrew with the contents of the andrew.csr file

	(*As of <span style="color:#5c7e3e">Kubernetes</span> 1.19, the apiVersion to use for CSR objects is certificates.k8s.io/v1)

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