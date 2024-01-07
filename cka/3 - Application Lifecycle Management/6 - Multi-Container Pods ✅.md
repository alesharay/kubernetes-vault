- The idea of decoupling large <span style="color:#5c7e3e">monolith</span> applications into smaller subcomponents is known as <span style="color:#5c7e3e">microservices</span>

- <span style="color:#5c7e3e">Microservices</span> enable us to develop and deploy a set of independent, small and reusable code

- <span style="color:#5c7e3e">Microservices</span> help us scale up, down and modify each [[10 - Services ✅|service]] as required

- At times, you may need two [[10 - Services ✅|services]] to work together such as a web server and a logging [[10 - Services ✅|service]]
	- Even though you want them to be deployed separately, you still want them to be paired together
	- This is the scenario where you would want <b><span style="color:#d46644">multiple containers</span></b> in a [[7 - Pods ✅|pod]]

- <b><span style="color:#d46644">Multiple containers</span></b> in a [[7 - Pods ✅|pod]] means they will be both created and destroyed together
	- They also share the same network space which means they can refer to each other as localhost, and they have access to the same [[3 - Volumes ✅|storage volumes]]

- To create a <b><span style="color:#d46644">multi-container pod</span></b>, add the new [[7 - Pods ✅|container]] information to the [[7 - Pods ✅|pod]] definition file

- Remember that the [[7 - Pods ✅|containers]] section in the <i><span style="color:#5c7e3e">spec</span></i> section is an <i><span style="color:#5c7e3e">array</span></i>
	- This is to allow <b><span style="color:#d46644">multiple containers</span></b> in a [[7 - Pods ✅|pod]]

![[multi-1.png]]

## Multi-Container Pods Design Patterns

- There are 3 common patterns when it comes to designing <b><span style="color:#d46644">multi-container pods</span></b>
	1. <b><span style="color:#d46644">Sidecar Pattern</span></b>
		1. The sidecar pattern consists of the main application, i.e. the web application, and a helper container with a responsibility that is vital for your application but is not necessarily a part of the application and might not be needed for the main container to work.

			![[multi-2.png]]

	1. <b><span style="color:#d46644">Adapter Pattern</span></b>
		1. The adapter pattern is used to standardize the output by the primary container. Standardizing refers to formatting the output in a specific manner that fits the standards across your applications.

			![[multi-3.png]]

	1. <b><span style="color:#d46644">Ambassador Pattern</span></b>
		1. The ambassador design pattern is used to connect containers with the outside world. In this design pattern, the helper container can send network requests on behalf of the main application. It is nothing but a proxy that allows other containers to connect to a port on the localhost.

			![[multi-4.png]]

### Practice Problems

- Create a multi-container pod with 2 containers using the given information:
	- Name: `Yellow`
	- Container 1 Name: `lemon`
	- Container 1 Image: `busybox`
	- Container 2 Name: `gold`
	- Container 2 Image: `redis`

	`kubectl run yellow --image=lemon --dry-run=client -o yaml > yellow-pod.yaml`

- Add the remaining container details