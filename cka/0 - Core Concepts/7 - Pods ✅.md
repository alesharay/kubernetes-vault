- <i><span style="color:#d46644">Containers</span></i> are encapsulated into <span style="color:#5c7e3e">Kubernetes</span> objects known as <b><span style="color:#d46644">pods</span></b>

- A <b><span style="color:#d46644">pod</span></b> is a single instance of an **<i><span style="color:#477bbe">application</span></i>
	- This is the smallest object to be created in <span style="color:#5c7e3e">Kubernetes</span>

- The <b><span style="color:#d46644">pod</span></b> <i><span style="color:#5c7e3e">spec</span></i> contains a <i><span style="color:#5c7e3e">list</span></i> of <i><span style="color:#5c7e3e">dictionaries</span></i> called <span style="color:red">containers</span>

- The <span style="color:red">kubectl get pods</span> command helps us see a list of <b><span style="color:#d46644">pods</span></b> in the [[0 - Core Concepts Intro ✅|cluster]]

- A single <b><span style="color:#d46644">pod</span></b> can have [[6 - Multi-Container Pods ✅|multiple containers]]
	- Except for the fact that they are normally not <i><span style="color:#d46644">containers</span></i> of the same kind

- If the <span style="color:red">kubectl get pods</span> command shows [[6 - Multi-Container Pods ✅|multiple containers]] for the <b><span style="color:#d46644">pod</span></b>, the <span style="color:red">kubectl describe pod</span> command will have separate information for each <i><span style="color:#d46644">container</span></i>
	- This includes separate <i><span style="color:#d46644">container</span></i> images

![[pods.png]]

- When we define what <i><span style="color:#d46644">containers</span></i> a <b><span style="color:#d46644">pod</span></b> consists of, the <i><span style="color:#d46644">containers</span></i> by default will have the same storage, network space, and fate

- The **READY** column of the <span style="color:red">kubectl get pod</span> command shows the number of running <i><span style="color:#d46644">containers</span></i> in the <b><span style="color:#d46644">pod</span></b> followed by the total number of <i><span style="color:#d46644">containers</span></i> in the <b><span style="color:#d46644">pod</span></b>

![[pods1.png]]

- To scale up an <i><span style="color:#477bbe">application</span></i>, new <i><span style="color:#d46644">containers</span></i> are not added to existing <b><span style="color:#d46644">pods</span></b>; instead, new <b><span style="color:#d46644">pods</span></b> are added to existing or new [[0 - Core Concepts Intro ✅|nodes]]

- To create a <b><span style="color:#d46644">pod</span></b> imperatively, use the <span style="color:red">kubectl run [pod_name] --image=[pod_image]</span> command

		kubectl run nginx --image=nginx

- The <span style="color:red">kubectl run</span> command creates a <i><span style="color:#d46644">container</span></i> by creating a <b><span style="color:#d46644">pod</span></b>

- To delete a <b><span style="color:#d46644">pod</span></b>, use the <span style="color:red">kubectl delete pod [pod_name]</span> command

		kubectl delete pod nginx

- To inspect a <b><span style="color:#d46644">pod</span></b>, use the <span style="color:red">kubectl describe pod [pod_name]</span> command

		kubectl describe pod nginx

From "Kubernetes for Beginners"



## Practice Questions

1. How many pods are in the current namespace?
2. Create a new pod with the `nginx` image
3. What image was used to create the `kube-scheduler` pod in the `kube-system` namespace?
4. Create 3 more `nginx` pods
5. Which nodes are the pods placed on?
6. Create a pod named `broken-pod` with the image `agentx`
7. What is the status of the `broken-pod` pod?
8. Why do you think the `broken-pod` pod is in that status?
9. What does the `READY` column in the output of the `kubectl get pods` command indicate?
10. Change the image on the `broken-pod` pod to `nginx` using the `set image` command
11. Delete the `broken-pod` pod