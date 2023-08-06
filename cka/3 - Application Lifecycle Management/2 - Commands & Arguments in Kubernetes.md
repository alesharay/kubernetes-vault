- To append <b><span style="color:#d46644">arguments</span></b> to the <span style="color:red">docker run</span> <b><span style="color:#d46644">command</span></b>, add an <b><span style="color:#d46644">args</span></b> property (in array form) to the [[7 - Pods|containers]] property of the [[7 - Pods|pod]] definition file
	- To match the <span style="color:#5c7e3e">json</span> format, put each index on a new line in the [[7 - Pods|pod]] definition

![[commands-2-1.png]]

- The values in <b><span style="color:#d46644">args</span></b> will override the <b><span style="color:#d46644">CMD</span></b> instruction from the [[7 - Pods|container's]] <span style="color:#5c7e3e">Dockerfile</span>

- To override the <b><span style="color:#d46644">ENTRYPOINT</span></b> from the [[7 - Pods|container's]] <span style="color:#5c7e3e">Dockerfile</span>, add a <b><span style="color:#d46644">command</span></b> property (in array form) to the [[7 - Pods|containers]] property of the [[7 - Pods|pod]] definition file
	- To match the <span style="color:#5c7e3e">json</span> format, put each index on a new line in the [[7 - Pods|pod]] definition

![[commands-2-2.png]]

- Be sure not to mix the <b><span style="color:#d46644">command</span></b> and <b><span style="color:#d46644">args</span></b> property up. The <b><span style="color:#d46644">command</span></b> property in the [[7 - Pods|pod]] definition file does not correspond to the <b><span style="color:#d46644">CMD</span></b> instruction in the <span style="color:#5c7e3e">Dockerfile</span>
	- It corresponds with the <b><span style="color:#d46644">ENTRYPOINT</span></b> instruction

### Practice Problems

- Create a [[7 - Pods|pod]] with the ubuntu image to run a [[7 - Pods|container]] to sleep for 5000 seconds.

	`kubectl run unbuntu --image=ubuntu --dry-run=client -o yaml --command -- sleep 5000 > ubuntu-sleeper.yaml`