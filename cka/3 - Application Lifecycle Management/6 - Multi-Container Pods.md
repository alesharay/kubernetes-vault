- The idea of decoupling large monolith applications into smaller subcomponents is known as microservices

- Microservices enable us to develop and deploy a set of independent, small and reusable code

- Microservices help us scale up, down and modify each service as required

- At times, you may need two services to work together such as a webserver and a logging service

- Even though you want them to deployed separately, you still want them to be paired together
- This is the scenario where you would want multiple containers in a pod

- Multiple containers in a pod means they will be both created and destroyed together

- They also share the same network space which means they can refer to each other as localhost, and they have access to the same storage volumes

- To create a multi-container pod, add the new container information to the pod definition file

- Remember that the containers section in the spec section is an array

- This is to allow multiple containers in a pod

![[multi-1.png]]

Multi-Container Pods Design Patterns

- There are 3 common patterns when it comes to designing multi-container pods

1. Sidecar pattern
2. Adapter pattern
3. Ambassador pattern

Practice Problems

- Create a multi-container pod with 2 containers using the given information:

- Name: Yellow
- Container 1 Name: lemon
- Container 1 Image: busybox
- Container 2 Name: gold
- Container 2 Image: redis

		kubectl run yellow --image=lemon --dry-run=client -o yaml > yellow-pod.yaml

- Add the remaining container details