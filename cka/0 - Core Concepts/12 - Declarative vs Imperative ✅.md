- An <b><span style="color:#d46644">imperative</span></b> approach is specifying what to do and exactly how to do it
	- A set of step-by-step instructions for <span style="color:#5c7e3e">provisioning</span> an <i><span style="color:#477bbe">infrastructure</span></i> is <b><span style="color:#d46644">imperative</span></b>

![[declarative-imperative-1.png]]

- In the <span style="color:#5c7e3e">Kubernetes</span> world, the <b><span style="color:#d46644">imperative</span></b> approach to <span style="color:#5c7e3e">provisioning</span> <i><span style="color:#477bbe">infrastructure</span></i> is using commands such as
	- kubectl run - to create a pod
	- kubectl create - to create a resource
	- kubectl create -f - creates an object from a definition file imperatively
	- kubectl expose - to create a service and expose it to the deployment
	- kubectl edit - to edit resources
	- kubectl scale - to scale the application up or down
	- kubectl set image - to change the image used for the pods
	- kubectl replace -f - to edit an object from a definition file imperatively
	- kubectl delete  - to delete an object
- These all say specifically how to bring the <i><span style="color:#477bbe">infrastructure</span></i> to our needs

- NOTE: The <b><span style="color:#d46644">imperative</span></b> approach will be useful during the <span style="color:#5c7e3e">certification exams</span> as we don't have to deal with YAML files
	- However, they're limited in functionality and will require forming long and complex commands for advanced use-cases such as creating a [[6 - Multi-Container Pods ✅|multi-container pods]] or [[9 - Deployments ✅|deployments]]

- <b><span style="color:#d46644">imperative</span></b> commands are run once and then forgotten
	- They're only available in the session history of the user who ran the commands
		- This makes it difficult for another user to figure out how the objects were created

- Creating ***files*** makes it easy to keep track of the <i><span style="color:#d46644">live</span></i> object of the objects

- When using the <span style="color:red">kubectl edit</span> command, we get additional fields, such as the <i><span style="color:#477bbe">status</span></i> field in what is called the <b><span style="color:#d46644">live object</span></b>
	- This stores the <i><span style="color:#477bbe">status</span></i> of the object
	- It's important to note that this is not the <span style="color:#5c7e3e">local</span> definition file used to create the object

- There are differences between the <b><span style="color:#d46644">live object</span></b> which contains the <i><span style="color:#477bbe">status</span></i>, and the <span style="color:#5c7e3e">local definition</span> file used to create the object

- When using the <span style="color:red">kubectl edit</span> command and editing the <b><span style="color:#d46644">live object</span></b>, the changes are not recorded anywhere
	- When new changes are made using the <span style="color:red">kubectl edit</span> command, there is no indication that previous changes used by the <span style="color:red">kubectl edit</span> command were made

- A better approach is to edit the <span style="color:#5c7e3e">local definition</span> file and use the <span style="color:red">kubectl replace</span> command to update the object

- If the object does not exist when using the <span style="color:red">kubectl replace</span> command, you will encounter a "conflict" error

- If the object already exists when you use the <span style="color:red">kubectl create</span> command, you will encounter an error that the object "already exists"

- The <b><span style="color:#d46644">imperative</span></b> approach is very taxing as an <i><span style="color:#477bbe">administrator</span></i> as you must always be aware of the current configuration

- By default, as soon as a command is run, the <span style="color:#5c7e3e">resource</span> will be created.
- If you simply want to test your command, use the <span style="color:red">--dry-run=client</span> option

	- This will <u>not</u> create the <span style="color:#5c7e3e">resource</span>; instead, it will tell you whether the <span style="color:#5c7e3e">resource</span> can be created and if your command is correct

![[declartive-imperative.png]]

- The <span style="color:red">-o FILE_TYPE</span> option will output the <span style="color:#5c7e3e">resource</span> definition in the format for the chosen file type
	- e.g. yaml, json

### DECLARATIVE

![[declarative-imperative-2.png]]

- A <b><span style="color:#d46644">declarative</span></b> approach is specifying what to do but not how to do it
	- A set of requirements without any instructions on how to <span style="color:#5c7e3e">provision</span> them is <b><span style="color:#d46644">declarative</span></b>
		- How everything is done is decided by the system

- Orchestration tools, such as Ansible, Terraform, puppet, and Chef use a <b><span style="color:#d46644">declarative</span></b> approach

- The <b><span style="color:#d46644">declarative</span></b> approach to <span style="color:#5c7e3e">provisioning</span> is creating a set of files that defines the <i><span style="color:#477bbe">desired state</span></i> of the applications and services on a <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]] and letting the system decide how to <span style="color:#5c7e3e">provision</span> them
	- A single command, <span style="color:red">kubectl apply -f</span> is needed for this approach
- This approach looks at the <span style="color:#5c7e3e">existing configuration</span> to bring the <i><span style="color:#477bbe">infrastructure</span></i> to its <i><span style="color:#477bbe">desired state</span></i>

- The <span style="color:red">kubectl apply -f</span> command is intelligent enough to know details such as if the object already exists and the existing <i><span style="color:#477bbe">status</span></i> of it

- The <span style="color:red">kubectl apply -f</span> command is used for all necessary changes such as <i><span style="color:#477bbe">create</span></i> and <i><span style="color:#477bbe">edit</span></i>

#### EXAM TIPS

- Use the <b><span style="color:#d46644">imperative</span></b> approach to save time as much as possible
- Practice the <b><span style="color:#d46644">imperative</span></b> commands as much as possible to be prepared
- Use an object definition file for complex tasks such as [[6 - Multi-Container Pods ✅|multi-container pods]]
	- Use the <b><span style="color:#d46644">declarative</span></b> approach when dealing with files, just using the <span style="color:red">kubectl apply -f</span> command to make changes

### IMPERATIVE EXAMPLES

\<\<\< POD \>\>\>

- **Create an NGINX pod**

	`kubectl run nginx --image=nginx`

- **Generate nginx POD manifest yaml file (-o yaml). Don't create it (--dry-run)**

	`kubectl run nginx --image=nginx --dry-run=client -o yaml >> nginx-pod.yaml`

\<\<\< DEPLOYMENT \>\>\>

- **Create an nginx deployment**

	`kubectl create deployment nginx --image=nginx`

- **Generate deployment yaml file (-o yaml). Don't create it (--dry - run)**

	`kubectl create deployment --image=nginx nginx --dry-run=client -o yaml >> nginx-deployment.yaml`

- **Generate deployment with 4 replicas**

	`kubectl create deployment nginx --image=nginx --replicas=4`

- **You can also scale a deployment using the scale command**

	`kubectl scale deployment nginx --replicas=4`

	- **Another way to do this is to save the yaml definition to a file and modify it**
		- You can then update the yaml file directly

\<\<\< SERVICE \>\>\>

- **Create a service named redis-service of type ClusterIP to expose pod redis on port 6379**

	`kubectl expose pod redis --port=6379 --name redis-service`

	- This will automatically use the pods labels as selectors

        OR

	`kubectl create service clusterip redis-service --tcp=6379:6379`

	- This will not use the pods labels as selectors
	- Instead it will assume selectors as app=redis-service. 
	- [You cannot pass in selectors as an option.](https://github.com/kubernetes/kubernetes/issues/46191) So it does not work very well if your pod has a different label set. So generate the file and modify the selectors before creating the service

- **Create a service named nginx of type NodePort to expose pod nginx's port 80 to port 30080 on the nodes**

	`kubectl expose pod nginx --type=NodePort --port=80 --name=nginx --dry-run=client -o yaml`

	- This will automatically use the pod’s labels as selectors, [but you cannot specify the node port](https://github.com/kubernetes/kubernetes/issues/25478).
	- You have to generate a definition file and then add the node port in manually before creating the service with the pod.

		OR

	`kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml`

	- This will not use the labels pods as selectors

	- <span style="color:#6f6e6e"><i>Both the above commands have their own challenges.</i></span>
	- <span style="color:#6f6e6e"><i>While one of them cannot accept a selector the other cannot accept a node port.</i></span>
	- <span style="color:#6f6e6e"><i>The recommendation is going with the `kubectl expose` command.</i></span>
	- <span style="color:#6f6e6e"><i>If you need to specify a node port, generate a definition file using the same command and manually input the nodeport before creating the service.</i></span>
	