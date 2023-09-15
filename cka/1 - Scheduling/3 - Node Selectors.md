- <span style="color:#5c7e3e">Limitations</span> can be set on [[7 - Pods ✅|pods]] so that they only run on the [[0 - Core Concepts Intro ✅|nodes]] that can handle their resource requirements

- There are two ways to add <span style="color:#5c7e3e">limitations</span> on [[7 - Pods ✅|pods]]:
	- <b><span style="color:#d46644">NodeSelectors</span></b>
		- Add a <b><span style="color:#d46644">nodeSelector</span></b> property to the [[7 - Pods ✅|pod]] definition file
		- The <b><span style="color:#d46644">nodeSelector</span></b> property is a child of the <i><span style="color:#477bbe">spec</span></i> section
		- Match the [[1 - Labels & Selectors ✅|labels]] assigned to the [[0 - Core Concepts Intro ✅|nodes]] that you want to specify in this section
			- This means you must have [[1 - Labels & Selectors ✅|labeled]] the [[0 - Core Concepts Intro ✅|nodes]] prior to creating the [[7 - Pods ✅|pod]] that you want to add the <b><span style="color:#d46644">nodeSelector</span></b> on

![[node-selector-1.png]]

- [[4 - Node Affinity|NodeAffinity]] (we'll cover this next)

- To [[1 - Labels & Selectors ✅|label]] a [[0 - Core Concepts Intro ✅|nodes]], use the following command

	`kubectl label nodes NODE_NAME LABEL_KEY=LABEL_VALUE`

- When the requirements for placing [[7 - Pods ✅|pods]] on [[0 - Core Concepts Intro ✅|nodes]] are more complicated (ie. Can go on any [[0 - Core Concepts Intro ✅|node]] that doesn't have specific requirements, or can go on [[0 - Core Concepts Intro ✅|nodes]] with multiple requirements), you would use the [[4 - Node Affinity|nodeAffinity]] section