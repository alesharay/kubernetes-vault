- This lesson will cover <b><i><span style="color:#d46644">image</span></i></b> names and configuring [[7 - Pods|pods]] to use <b><i><span style="color:#d46644">images</span></i></b> from secure repositories

- When you use the <span style="color:#5c7e3e">nginx</span> <b><i><span style="color:#d46644">image</span></i></b>, it's actually **library**/<span style="color:#5c7e3e">nginx</span>

- When setting the <b><i><span style="color:#d46644">image</span></i></b> name, if you don't define a <i><span style="color:#477bbe">user</span></i> or account name (the first part of the <b><i><span style="color:#d46644">image</span></i></b> name), <span style="color:#5c7e3e">Kubernetes</span> assumes it to be "**library**"
	- **Library** is the name of the default account where <span style="color:#5c7e3e">Docker's</span> official <b><i><span style="color:#d46644">images</span></i></b> are stored

![[images-security-1.png]]

- <span style="color:#5c7e3e">Docker's</span> official <b><i><span style="color:#d46644">images</span></i></b> promote best practices and are maintained by a dedicated team who are responsible for reviewing and publishing the <b><i><span style="color:#d46644">images</span></i></b>

- If you have your own <span style="color:#5c7e3e">Docker</span> account , the pattern would be similar: your-name/image-name

- For <b><i><span style="color:#d46644">images</span></i></b> in the <span style="color:#5c7e3e">Docker</span> <b><i><span style="color:#d46644">registry</span></i></b>, they are both stored and pulled from the <span style="color:#5c7e3e">docker.io</span> [[0.2 - DNS|DNS]]

![[images-security-2.png]]

- There are many other popular <b><i><span style="color:#d46644">image registries</span></i></b> (ie. <span style="color:#5c7e3e">Google</span>: <span style="color:#5c7e3e">gcr.io</span>)

- Public <b><i><span style="color:#d46644">image repositories</span></i></b> give anyone access and download privileges

- When you have applications built in-house that shouldn't be made available to the public, hosting an internal <b><i><span style="color:#d46644">private registry</span></i></b> may be a good solution

- Many cloud service providers such as <span style="color:#5c7e3e">AWS</span>, <span style="color:#5c7e3e">GCP</span> or <span style="color:#5c7e3e">Azure</span> provide a <b><i><span style="color:#d46644">private registry</span></i></b> by default

- On any of the public <b><i><span style="color:#d46644">image registries</span></i></b>, you may choose to make your <b><i><span style="color:#d46644">registry registry</span></i></b> so that it is only accessible using a set of credentials

- From <span style="color:#5c7e3e">Docker's</span> perspective, to run a container using a <b><i><span style="color:#d46644">private image</span></i></b>, you must first login to your <b><i><span style="color:#d46644">private registry</span></i></b> using the `docker login` command followed by the <b><i><span style="color:#d46644">private registry</span></i></b> name
	- Then input your credentials

![[images-security-3.png]]

![[images-security-4.png]]

- Once successfully logged into the <span style="color:#5c7e3e">Docker</span> <b><i><span style="color:#d46644">private registry</span></i></b>, run the application using the <b><i><span style="color:#d46644">image</span></i></b> from the <b><i><span style="color:#d46644">private registry</span></i></b>

![[images-security-5.png]]

- In the [[7 - Pods|pod]] definition file, to use an <b><i><span style="color:#d46644">image</span></i></b> from a <b><i><span style="color:#d46644">private registry</span></i></b>, we use the full name of the <b><i><span style="color:#d46644">registry</span></i></b> along with the <b><i><span style="color:#d46644">image</span></i></b> name

![[images-security-6.png]]

- Within <span style="color:#5c7e3e">Kubernetes</span>, <b><i><span style="color:#d46644">images</span></i></b> are pulled by the [[0 - Core Concepts Intro|container runtime]]  (<span style="color:#5c7e3e">Docker</span>) on the [[0 - Core Concepts Intro|worker nodes]]

- In order for <span style="color:#5c7e3e">Kubernetes</span> to get the credentials to access the <b><i><span style="color:#d46644">private registry</span></i></b>, thus [[1 - Authentication|authenticating]] the <i><span style="color:#477bbe">user</span></i> to the <b><i><span style="color:#d46644">private registry</span></i></b> from within <span style="color:#5c7e3e">Kubernetes</span>, a [[5 - Secrets|secret]] object is created with the credentials in it
	- The [[5 - Secrets|secret]] is of type <span style="color:#5c7e3e">docker-registry</span>

- <span style="color:#5c7e3e">Docker-registry</span> is a built-in [[5 - Secrets|secret]] type that was built for storing <span style="color:#5c7e3e">Docker</span> credentials

- Within the <b><i><span style="color:#d46644">image</span></i></b> [[5 - Secrets|pull secret]], specify the <b><i><span style="color:#d46644">private registry</span></i></b> name, the username, the password, and the email address that has access to <b><i><span style="color:#d46644">pull images</span></i></b>

![[images-security-7.png]]

- The [[5 - Secrets|secret]] with the <span style="color:#5c7e3e">Docker</span> <b><i><span style="color:#d46644">private registry</span></i></b> credentials is specified in the [[7 - Pods|pod]] definition file using the <span style="color:#5c7e3e">imagePullSecret</span> property
	- This a child to the <span style="color:#5c7e3e">spec</span> property

![[images-security-8.png]]

- When the [[7 - Pods|pod]] is created with the <b><i><span style="color:#d46644">imagePullSecret</span></i></b> details, [[5 - Kubelet|kubelet]] (on the [[0 - Core Concepts Intro|worker nodes]]) uses the [[5 - Secrets|secret]] object to <b><i><span style="color:#d46644">pull images</span></i></b>

### Practice Problems

- What secret type must we choose for docker-registry?

		docker-registry

- What is the image used on any application in the cluster?

		kubectl describe pod <pod name> | grep -I image

	(jot down the image name)