- Every [[7 - Pods ✅|pod]] has a field called <b><span style="color:#d46644">nodeName</span></b> that, by default, is not set
	- This field is added automatically by <span style="color:#5c7e3e">Kubernetes</span>

![[scheduling-intro-1.png]]

- The [[9 - Multiple Schedulers|scheduler]] goes through all of the [[7 - Pods ✅|pods]] and looks for those that do not have this property set
	- These are candidates for <b><span style="color:#d46644">scheduling</span></b>
	- It then identifies the correct [[0 - Core Concepts Intro ✅|node]] for the [[7 - Pods ✅|pod]] by using the <b><span style="color:#d46644">scheduling</span></b> algorithm

- In a [[0 - Core Concepts Intro ✅|cluster]], [[0 - Core Concepts Intro ✅|nodes]] that meet the <b><span style="color:#d46644">scheduling</span></b> requirements for a [[7 - Pods ✅|pod]] are called <u><span style="color:#5c7e3e">feasible</span></u> [[0 - Core Concepts Intro ✅|nodes]]

- The [[4 - Kube Scheduler ✅|scheduler]] <b><span style="color:#d46644">schedules</span></b> the [[7 - Pods ✅|pod]] on the [[0 - Core Concepts Intro ✅|node]] by setting the <b><span style="color:#d46644">nodeName</span></b> property to the name of the [[0 - Core Concepts Intro ✅|node]]
	- This is done by creating a <b><span style="color:#d46644">binding object</span></b>

- If there is no [[4 - Kube Scheduler ✅|scheduler]] to monitor and <b><span style="color:#d46644">schedule</span></b> [[7 - Pods ✅|pods]] to be placed on [[0 - Core Concepts Intro ✅|nodes]], then the pods remain in [[7 - Pods ✅|pods]] <u><span style="color:#5c7e3e">pending</span></u> state

- [[7 - Pods ✅|Pods]] can be manually assigned to [[0 - Core Concepts Intro ✅|nodes]] by manually setting the <b><span style="color:#d46644">nodeName</span></b> property

- The <b><span style="color:#d46644">nodeName</span></b> property can only be set at creation time
	- <span style="color:#5c7e3e">Kubernetes</span> doesn't allow you to set the <b><span style="color:#d46644">nodeName</span></b> property of an existing [[7 - Pods ✅|pod]]

- Another way to assign a [[7 - Pods ✅|pod]] to a [[0 - Core Concepts Intro ✅|node]] is by creating a <b><span style="color:#d46644">binding object</span></b> and sending a <span style="color:#5c7e3e">POST</span> request to the [[7 - Pods ✅|pod]] <b><span style="color:#d46644">binding</span></b> API (which mimics what the [[4 - Kube Scheduler ✅|scheduler]] does)

![[scheduling-intro-2.png]]

- In the <b><span style="color:#d46644">binding object</span></b>, you specify a target [[0 - Core Concepts Intro ✅|node]] by using the name of the [[0 - Core Concepts Intro ✅|node]]
	- Then send the <span style="color:#5c7e3e">POST</span> request
	- You must convert the <span style="color:#5c7e3e">YAML</span> file for the <b><span style="color:#d46644">binding object</span></b> to <span style="color:#5c7e3e">JSON</span> for the post request
		- then use the <span style="color:#5c7e3e">JSON</span> data in the <span style="color:#5c7e3e">POST</span> request

Using <span style="color:#5c7e3e">curl</span>

![[scheduling-intro-3.png]]

Using an <span style="color:#5c7e3e">http client</span> (like postman)

![[scheduling-intro-4.png]]

- The [[4 - Kube Scheduler ✅|scheduler]] runs as a [[7 - Pods ✅|pod]] in the kube-system [[11 - Namespaces|namespace]]

- When a [[7 - Pods ✅|pod]] is in a <span style="color:#5c7e3e">pending</span> state and after inspecting, there is no other issue in the events, review the [[4 - Kube Scheduler ✅|scheduler]] [[7 - Pods ✅|pod]]

- If you list all of the [[7 - Pods ✅|pods]] and the [[4 - Kube Scheduler ✅|scheduler]] is not there, that will mean the [[7 - Pods ✅|pods]] will not be <b><span style="color:#d46644">scheduled</span></b> automatically

![[scheduling-intro-5.png]]