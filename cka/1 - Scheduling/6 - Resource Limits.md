- Whenever a [[7 - Pods ✅|pod]] is placed on a [[0 - Core Concepts Intro ✅|node]] it consumes <b><span style="color:#d46644">resources</span></b> available to the [[0 - Core Concepts Intro ✅|node]] (<span style="color:#5c7e3e">CPU</span>, <span style="color:#5c7e3e">memory</span>, <span style="color:#5c7e3e">disk space</span>, etc…)

- To recap, the [[4 - Kube Scheduler ✅|scheduler]] takes into consideration the amount of <b><span style="color:#d46644">resources</span></b> required for a specific [[7 - Pods ✅|pod]] and those available on the [[0 - Core Concepts Intro ✅|nodes]]

- If a [[0 - Core Concepts Intro ✅|node]] doesn't have the required <b><span style="color:#d46644">resources</span></b> for a [[7 - Pods ✅|pod]], the [[7 - Pods ✅|pod]] is placed on a [[0 - Core Concepts Intro ✅|node]] that does

- If there are no sufficient <b><span style="color:#d46644">resources</span></b> available on any of the [[0 - Core Concepts Intro ✅|nodes]], the [[4 - Kube Scheduler ✅|scheduler]] will hold back the [[7 - Pods ✅|pod]] and it will remain in the pending state
	- The events section (when you describe the [[7 - Pods ✅|pod]]) will explain what <b><span style="color:#d46644">resource</span></b> is insufficient

![[resource-limits-1.png]]

- By default, <span style="color:#5c7e3e">Kubernetes</span> assumes a [[7 - Pods ✅|pod]] requires:
	- .5 <span style="color:#5c7e3e">CPU</span>
	- 256 mebibytes of <span style="color:#5c7e3e">memory</span>

- This is known as the <b><span style="color:#d46644">resource</span></b> request for a <i><span style="color:#477bbe">container</span></i>
	- This is the minimum amount of <span style="color:#5c7e3e">CPU</span> or <span style="color:#5c7e3e">memory</span> requested by the <i><span style="color:#477bbe">container</span></i> when the [[4 - Kube Scheduler ✅|scheduler]] tries to place a [[7 - Pods ✅|pod]] on a [[0 - Core Concepts Intro ✅|node]]

- If your [[7 - Pods ✅|pod]]s require more than the minimum amount of <b><span style="color:#d46644">resources</span></b>, they can be specified in the [[7 - Pods ✅|pod]] or [[9 - Deployments ✅|deployment]] definition files

- In a simple [[7 - Pods ✅|pod]] definition file, add a section called <b><span style="color:#d46644">resources</span></b> , under which add requests and specify the new values for <span style="color:#5c7e3e">memory</span> and <span style="color:#5c7e3e">CPU</span> usage

![[resource-limits-2.png]]

![[resource-limits-3.png]]

![[resource-limits-4.png]]

- For the [[7 - Pods ✅|pod]] to pick up those defaults, you must have first set those as default values for request and <b><span style="color:#d46644">limit</span></b> by creating a <b><span style="color:#d46644">LimitRange</span></b> in that [[11 - Namespaces|namespace]]

![[resource-limits-5.png]]

![[resource-limits-6.png]]

- By default, <span style="color:#5c7e3e">Kubernetes</span> sets a <b><span style="color:#d46644">limit</span></b> of 1 v<span style="color:#5c7e3e">CPU</span>.
	- Unless specifically specified otherwise, your [[7 - Pods ✅|pod]] will not be able to occupy more than that <b><span style="color:#d46644">limit</span></b>

- By default, <span style="color:#5c7e3e">Kubernetes</span> sets a <b><span style="color:#d46644">limit</span></b> of 512 Mi (mebibytes) of <span style="color:#5c7e3e">memory</span> on <i><span style="color:#477bbe">containers</span></i>

- To change the default <b><span style="color:#d46644">limits</span></b>, add a <b><span style="color:#d46644">limits</span></b> section under the <b><span style="color:#d46644">resources</span></b> section of a [[7 - Pods ✅|pod]] or [[9 - Deployments ✅|deployment]] definition file

![[resource-limits-7.png]]

- When a [[7 - Pods ✅|pod]] tries to exceed the <b><span style="color:#d46644">resources</span></b> specified by the <b><span style="color:#d46644">limits</span></b>,
	- <span style="color:#5c7e3e"><span style="color:#5c7e3e">CPU</span></span> - <span style="color:#5c7e3e">Kubernetes</span> throttles (kills) the extra <span style="color:#5c7e3e">CPU</span> so that it does not go beyond the specified <b><span style="color:#d46644">limit</span></b>
		- A <i><span style="color:#477bbe">container</span></i> cannot use more <span style="color:#5c7e3e">CPU</span> <b><span style="color:#d46644">resources</span></b> than its <b><span style="color:#d46644">limit</span></b>
	- <span style="color:#5c7e3e">Memory</span> - the [[7 - Pods ✅|pod]] will be terminated if a [[7 - Pods ✅|pod]] tries to consume more <span style="color:#5c7e3e">memory</span> than its <b><span style="color:#d46644">limit</span></b> constantly
		- A <i><span style="color:#477bbe">container</span></i> CAN use more <span style="color:#5c7e3e">memory</span> <b><span style="color:#d46644">resources</span></b> than its <b><span style="color:#d46644">limits</span></b>
