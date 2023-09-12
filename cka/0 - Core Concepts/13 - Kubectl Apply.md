- The <span style="color:red">kubectl apply</span> command takes the following into consideration before making a decision on what changes are to be made
	- The local object definition file
	- The live object configuration
	- The last applied configuration

- When you run the <span style="color:red">kubectl apply</span> command, if the <i><span style="color:#477bbe">object</span></i> does not already exist, it will be created

- When the <i><span style="color:#477bbe">object</span></i> is created, a <b><span style="color:#d46644">live object configuration</span></b> similar to what we created locally is created but with additional fields to store the status of the <i><span style="color:#477bbe">objects</span></i>

- This is how <span style="color:#5c7e3e">Kubernetes</span> internally stores information about the <i><span style="color:#477bbe">object</span></i> no matter what approach is used to create the <i><span style="color:#477bbe">object</span></i>

- When using the <span style="color:red">kubectl apply -f</span> approach to create an <i><span style="color:#477bbe">object</span></i>, it does something a bit more…
	- The values in the <b><span style="color:#d46644">local object definition</span></b> file is converted to JSON and is then stored as the <b><span style="color:#d46644">last applied configuration</span></b>

- Going forward, for any changes applied to the <i><span style="color:#477bbe">object</span></i> , all three (<b><span style="color:#d46644">local object definition</span></b>, live object config, last applied config) are compared in order to identify what changes are to be made on the <b><span style="color:#d46644">live object configuration</span></b>

- If a field was deleted, and then the <span style="color:red">kubectl apply</span> command is run, we see that the <b><span style="color:#d46644">last applied configuration</span></b> had the deleted field, but it's not present in the <b><span style="color:#d46644">local object definition</span></b> file. 
	- This would mean that the field would need to be removed from the <b><span style="color:#d46644">live object configuration</span></b>

- If a field is present in the <b><span style="color:#d46644">live object configuration</span></b>, but is not present in <b><span style="color:#d46644">local object definition</span></b> or <b><span style="color:#d46644">last applied configuration</span></b>, it will be left as is

- If a field is missing from the <b><span style="color:#d46644">local object definition</span></b>, but is present on the <b><span style="color:#d46644">last applied configuration</span></b>, this means that in the previous step or whenever the last time the <span style="color:red">kubectl apply -f</span> command was run, that field was there and is now being removed

- To summarize, the <b><span style="color:#d46644">last applied configuration</span></b> helps us figure out what fields have been removed from the <b><span style="color:#d46644">local object definition</span></b> file
	- That field is then removed from the <b><span style="color:#d46644">live object configuration</span></b>

![[kubectl-apply-1.png]]

- The <b><span style="color:#d46644">local object definition</span></b> file is found on our <i><span style="color:#477bbe">local system</span></i>
- The <b><span style="color:#d46644">live object configuration</span></b> is stored in Kubernetes <i><span style="color:#477bbe">memory</span></i>
- The <b><span style="color:#d46644">last applied configuration</span></b> is stored as a property in the <b><span style="color:#d46644">live object configuration</span></b> on the Kubernetes [[0 - Core Concepts Intro ✅|cluster]] itself as an <i><span style="color:#477bbe">annotation</span></i>

- The name of the <i><span style="color:#477bbe">annotation</span></i> for the <b><span style="color:#d46644">last applied configuration</span></b> is `kubectl.kubernetes.io/last-applied-configuration`

- This only appears when using the <span style="color:red">kubectl apply</span> command

	<span style="color:red">NOTE: remember not to mix imperative and declarative approaches. Once one approach is used, that should be the approach you stick to</span>