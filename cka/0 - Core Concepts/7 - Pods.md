- <i><span style="color:#477bbe">Containers</span></i> are encapsulated into <span style="color:#5c7e3e">Kubernetes</span> objects known as <b><span style="color:#d46644">pods</span></b>

- A <b><span style="color:#d46644">pod</span></b> is a single instance of an application
	- This is the smallest object to be created in <span style="color:#5c7e3e">Kubernetes</span>

- The <b><span style="color:#d46644">pod</span></b> spec contains a <i><span style="color:#5c7e3e">list</span></i> of <i><span style="color:#5c7e3e">dictionaries</span></i> called "<i><span style="color:#477bbe">containers</span></i>"

- The <span style="color:red">kubectl get pod</span> command helps us see a list of <b><span style="color:#d46644">pods</span></b> in the [[0 - Core Concepts Intro ✅|cluster]]

- A single <b><span style="color:#d46644">pod</span></b> can have multiple <i><span style="color:#477bbe">containers</span></i>
	- Except for the fact that they are normally not <i><span style="color:#477bbe">containers</span></i> of the same kind

- If the <span style="color:red">kubectl get pods</span> command shows multiple <i><span style="color:#477bbe">containers</span></i> for the <b><span style="color:#d46644">pod</span></b>, the <span style="color:red">kubectl describe pod</span> command will have separate information for each <i><span style="color:#477bbe">container</span></i>
	- This includes separate <i><span style="color:#477bbe">container</span></i> images

- Multi-<i><span style="color:#477bbe">container</span></i> <b><span style="color:#d46644">pods</span></b> are a rare use-case

- When we define what <i><span style="color:#477bbe">containers</span></i> a <b><span style="color:#d46644">pod</span></b> consists of, the <i><span style="color:#477bbe">containers</span></i>, by default, will have the same storage, network space and fate

- The READY column of the <span style="color:red">kubectl get pod</span> command shows the number of running <i><span style="color:#477bbe">containers</span></i> in the <b><span style="color:#d46644">pod</span></b> followed by the total number of <i><span style="color:#477bbe">containers</span></i> in the <b><span style="color:#d46644">pod</span></b>

- To scale up an application, new <i><span style="color:#477bbe">containers</span></i> are not added to existing <b><span style="color:#d46644">pods</span></b>; instead, new <b><span style="color:#d46644">pods</span></b> are added to existing or new [[0 - Core Concepts Intro ✅|nodes]]

- To create a <b><span style="color:#d46644">pod</span></b> imperatively, use the <span style="color:red">kubectl run [pod_name] --image=[pod_image]</span> command

		kubectl run nginx --image=nginx

- The <span style="color:red">kubectl run</span> command deploys a <i><span style="color:#477bbe">container</span></i> by creating a <b><span style="color:#d46644">pod</span></b>

- To delete a <b><span style="color:#d46644">pod</span></b>, use the <span style="color:red">kubectl delete pod [pod_name]</span> command

		kubectl delete pod nginx

- To inspect a <b><span style="color:#d46644">pod</span></b>, use the <span style="color:red">kubectl describe pod [pod_name]</span> command

		kubectl describe pod nginx

[From "Kubernetes for Beginners"](onenote:Kubernetes%20for%20Beginners.one#Pods&section-id={5D5A45D8-45DB-1442-8EBF-F2131933F0D4}&page-id={54255497-E41E-A247-9570-F8A54DBE7CCB}&end&base-path=https://d.docs.live.net/a8ff567768035d78/Documents/Kubernetes)