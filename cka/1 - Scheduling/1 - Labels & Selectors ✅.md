- <b><span style="color:#d46644">Labels</span></b> and <b><span style="color:#d46644">selectors</span></b> are a standard method of grouping things together

- The best way to group things together based on specific needs is by using <b><span style="color:#d46644">labels</span></b>

- <b><span style="color:#d46644">Labels</span></b> are properties attached to each item

- <b><span style="color:#d46644">Selectors</span></b> help you filter items by their <b><span style="color:#d46644">Labels</span></b>

- In <span style="color:#5c7e3e">Kubernetes</span>, there are several ways to group <i><span style="color:#477bbe">objects</span></i> together
	- By <i><span style="color:#477bbe">object</span></i> type ([[7 - Pods ✅|pod]], [[9 - Deployments ✅|deployment]], [[10 - Services ✅|service]], <i><span style="color:#477bbe">ingress</span></i>, ….)
	- By application (app1, app2, app3, …)
	- By functionality (front-end, back-end, web-service, app-server, …)
	- Etc…

- The <b><span style="color:#d46644">labels</span></b> section falls under the <i><span style="color:#477bbe">metadata</span></i> property

![[las-1.png]]

- You can add as many <b><span style="color:#d46644">labels</span></b> as you'd like

- Once the [[7 - Pods ✅|pods]] are created, use the <span style="color:red">kubectl get pods</span> command along with the <span style="color:red">--selector</span> option

![[las-2.png]]

- The <b><span style="color:#d46644">labels</span></b> under the [[9 - Deployments ✅|pod template]] section are the <b><span style="color:#d46644">labels</span></b> configured on the [[7 - Pods ✅|pod]]
- The <b><span style="color:#d46644">labels</span></b> under the <i><span style="color:#477bbe">metadata</span></i> section are specific to that object
	- To connect the [[7 - Pods ✅|pod]] to another <i><span style="color:#477bbe">object</span></i> (like deployment), use the <b><span style="color:#d46644">selector</span></b> section and give the <b><span style="color:#d46644">matchLabels</span></b> the same <b><span style="color:#d46644">labels</span></b> as the [[7 - Pods ✅|pods]] you want

- The <b><span style="color:#d46644">labels</span></b> for an object (other than [[7 - Pods ✅|pods]]) are used when you want to connect some other object with the source object

- While <b><span style="color:#d46644">labels</span></b> and <b><span style="color:#d46644">selectors</span></b> are used to group and select objects, <b><span style="color:#d46644">annotations</span></b> are used to record other details for informatory purposes