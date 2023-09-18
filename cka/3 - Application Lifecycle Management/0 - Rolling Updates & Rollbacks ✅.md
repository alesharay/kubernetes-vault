- When you first create a [[9 - Deployments ✅|deployment]], it triggers a <b><span style="color:#d46644">rollout</span></b>

- A new <b><span style="color:#d46644">rollout</span></b> creates a new [[9 - Deployments ✅|deployment]] <b><span style="color:#d46644">revision</span></b>

- In the future, when a <b><span style="color:#d46644">revision</span></b> is updated, a new <b><span style="color:#d46644">rollout</span></b> is triggered and a new [[9 - Deployments ✅|deployment]] <b><span style="color:#d46644">revision</span></b> is created
	- This helps keep track of the changes made to the [[9 - Deployments ✅|deployment]]
	- This also helps <b><span style="color:#d46644">rollback</span></b> to a previous version of [[9 - Deployments ✅|deployment]] if necessary

- You can see the status of the <b><span style="color:#d46644">rollout</span></b> by running the <span style="color:red">kubectl rollout status</span> command followed by the name of the [[9 - Deployments ✅|deployment]]

![[rolling-1.png]]

- To see the <b><span style="color:#d46644">revisions</span></b> and history of <b><span style="color:#d46644">rollout</span></b>, run the <span style="color:red">kubectl rollout history</span> command followed by the name of the [[9 - Deployments ✅|deployment]]

![[rolling-2.png]]

### How to update a deployment?

- One way to update a [[9 - Deployments ✅|deployment]] is to edit and modify the [[9 - Deployments ✅|deployment]] definition file
	- Once these changes are made, run the <span style="color:red">kubectl apply -f</span> command to apply the changes
	- A new <b><span style="color:#d46644">rollout</span></b> is then triggered and a new <b><span style="color:#d46644">revision</span></b> of the [[9 - Deployments ✅|deployment]] is created

- There are other ways to edit certain properties (e.g. <span style="color:red">kubectl set image</span> updates the image)

### How deployments perform upgrades under the hood

- When a new [[9 - Deployments ✅|deployment]] is created, it first creates a new [[8 - ReplicaSets ✅|replicaSet]] automatically
	- The [[8 - ReplicaSets ✅|replicaSet]] then creates the number of [[7 - Pods ✅|pods]] required to be alive at any given time

- When you upgrade the application, the [[9 - Deployments ✅|deployment]] object creates a new [[8 - ReplicaSets ✅|replicaSet]] under the hood and starts deploying the containers there while at the same time, taking down the containers in the existing [[8 - ReplicaSets ✅|replicaSet]], following a rolling update strategy

- To <b><span style="color:#d46644">rollback</span></b> a change, run the <span style="color:red">kubectl rollout undo</span> command followed by the name of the [[9 - Deployments ✅|deployment]]
	- Say, for instance, once you've upgraded your application, you realize something isn't right (ie. There is an issue with the new build of the app) so you would like to <b><span style="color:#d46644">rollback</span></b> the application to the previous working version;

- When you run the <span style="color:red">kubectl rollout undo</span> command, the <b><span style="color:#d46644">rollout</span></b> process goes in reverse, where the [[7 - Pods ✅|pods]] in the new [[8 - ReplicaSets ✅|replicaSet]] are terminated as the [[7 - Pods ✅|pods]] in the previous [[8 - ReplicaSets ✅|replicaSet]] are spun back up

![[rolling-3.png]]