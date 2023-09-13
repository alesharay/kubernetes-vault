- The idea of decoupling large <span style="color:#5c7e3e">monolith</span> applications into smaller subcomponents is known as <span style="color:#5c7e3e">microservices</span>

- <span style="color:#5c7e3e">Microservices</span> enable us to develop and deploy a set of independent, small and reusable code

- <span style="color:#5c7e3e">Microservices</span> help us scale up, down and modify each [[10 - Services ✅|service]] as required

- At times, you may need two [[10 - Services ✅|services]] to work together such as a webserver and a logging [[10 - Services ✅|service]]
	- Even though you want them to deployed separately, you still want them to be paired together
	- This is the scenario where you would want multiple [[7 - Pods ✅|containers]] in a [[7 - Pods ✅|pod]]

- Multiple [[7 - Pods ✅|containers]] in a [[7 - Pods ✅|pod]] means they will be both created and destroyed together
	- They also share the same network space which means they can refer to each other as localhost, and they have access to the same storage [[3 - Volumes|volumes]]

- To create a <b><span style="color:#d46644">multi-container pod</span></b>, add the new [[7 - Pods ✅|container]] information to the [[7 - Pods ✅|pod]] definition file

- Remember that the [[7 - Pods ✅|containers]] section in the <span style="color:#5c7e3e">spec</span> section is an <span style="color:#5c7e3e">array</span>
	- This is to allow multiple [[7 - Pods ✅|containers]] in a [[7 - Pods ✅|pod]]

![[multi-1.png]]

### <b><span style="color:#d46644">Multi-Container Pods</span></b> Design Patterns

- There are 3 common patterns when it comes to designing <b><span style="color:#d46644">multi-container pods</span></b>
	1. Sidecar pattern
	2. Adapter pattern
	3. Ambassador pattern

### Practice Problems

- Create a multi-container pod with 2 containers using the given information:
	- Name: `Yellow`
	- Container 1 Name: `lemon`
	- Container 1 Image: `busybox`
	- Container 2 Name: `gold`
	- Container 2 Image: `redis`

	`kubectl run yellow --image=lemon --dry-run=client -o yaml > yellow-pod.yaml`

- Add the remaining container details