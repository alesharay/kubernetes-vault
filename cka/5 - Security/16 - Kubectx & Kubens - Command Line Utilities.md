- In a real “live” <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]] implemented for production, there could be a possibility of often switching between a large number of [[11 - Namespaces ✅|namespaces]] and [[0 - Core Concepts Intro ✅|clusters]]
	- This can quickly become a confusing and overwhelming task if you had to rely on [[13 - Kubectl Apply|kubectl]] alone

- This is where command line tools such as <b><i><span style="color:#d46644">kubectx</span></i></b> and <b><i><span style="color:#d46644">kubens</span></i></b> come in to picture
	- Reference:  [https://github.com/ahmetb/kubectx](https://github.com/ahmetb/kubectx)

### Kubectx:

- With this tool, you don’t have to make use of lengthy <span style="color:red">kubectl config</span> commands to switch between contexts.
	- This tool is particularly useful to switch context between [[0 - Core Concepts Intro ✅|clusters]] in a [[0 - Core Concepts Intro ✅|multi-cluster]] environment

- Installation:

		sudo git clone [https://github.com/ahmetb/kubectx /opt/kubectx](https://github.com/ahmetb/kubectx%20/opt/kubectx)
		sudo ln -s /opt/kubectx/kubectx /usr/local/bin/kubectx

#### Syntax:

- To list all contexts:

		kubectx

- To switch to a new context:

		kubectx

- To switch back to the previous context:

		kubectx –

- To see the current context:

		kubectx -c

### Kubens:

- This tool allows <i><span style="color:#477bbe">users</span></i> to switch between [[11 - Namespaces ✅|namespaces]] quickly with a simple command.

- Installation:

		sudo git clone [https://github.com/ahmetb/kubectx /opt/kubectx](https://github.com/ahmetb/kubectx%20/opt/kubectx)
		sudo ln -s /opt/kubectx/kubens /usr/local/bin/kubens

#### Syntax:

- To switch to a new [[11 - Namespaces ✅|namespace]]:

		kubens

- To switch back to previous [[11 - Namespaces ✅|namespace]]:

		kubens –