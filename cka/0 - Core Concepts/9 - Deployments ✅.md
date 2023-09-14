- <b><span style="color:#d46644">Deployments</span></b> provide us with the capability to upgrade underlying Instances using <i><span style="color:#477bbe">rolling updates</span></i>, make changes, and pause and resume changes as required

- <b><span style="color:#d46644">Deployments</span></b> automatically create [[8 - ReplicaSets ✅|ReplicaSets]]

- <b><span style="color:#d46644">Deployment</span></b> is a higher-level concept that manages [[8 - ReplicaSets ✅|ReplicaSets]] and provides [[12 - Declarative vs Imperative ✅|declarative]] updates to [[7 - Pods ✅|pods]] along with a lot of other useful features

- To update the [[11 - Image Security|image]] with the <span style="color:red">kubectl set image</span> command, you'll need to give the [[11 - Image Security|image]] name and tag

	`kubectl set image deployment/<deployment_name> <container_name>=<image:tag>`

	- If multiple <i><span style="color:#d46644">containers</span></i>, separate by commas ( , )
	- If all <i><span style="color:#d46644">containers</span></i>, use star ( * ) for the container_name

- To update an [[11 - Image Security|image]] in the definition files, you can edit the file directly and use the <span style="color:red">kubectl apply -f</span> command when the changes are saved
	- or you can use the <span style="color:red">kubectl set image</span> command

- Using the <span style="color:red">--record</span> flag when creating a new <b><span style="color:#d46644">deployment</span></b> will take note of any change requests so that in the status check, you can see the reason for the change

[From Kubernetes for Beginners - Deployments](onenote:Kubernetes%20for%20Beginners.one#Deployments&section-id={5D5A45D8-45DB-1442-8EBF-F2131933F0D4}&page-id={979D8C6B-5CB4-1D44-A98F-45B24F42B81B}&end&base-path=https://d.docs.live.net/a8ff567768035d78/Documents/Kubernetes)