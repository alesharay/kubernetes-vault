- Given a [[7 - Pods ✅|pod]] definition file, to set an <b><span style="color:#d46644">environment variable</span></b> to use with the <span style="color:#5c7e3e">Dockerfile</span>, add an <b><span style="color:#d46644">env</span></b> property that consists of an <span style="color:#5c7e3e">array</span> of <span style="color:#5c7e3e">dictionaries</span> with properties <span style="color:#5c7e3e">name</span> and <span style="color:#5c7e3e">value</span>
	- The <span style="color:#5c7e3e">name</span> is the name of the <b><span style="color:#d46644">environment variable</span></b> made available to the [[7 - Pods ✅|container]]
	- The <span style="color:#5c7e3e">value</span> is the value of the <b><span style="color:#d46644">environment variable</span></b> made available to the [[7 - Pods ✅|container]]

![[configure-1.png]]

- Other ways to set the <b><span style="color:#d46644">environment variables</span></b> is to use <i><span style="color:#477bbe">configMaps</span></i> and <i><span style="color:#477bbe">secrets</span></i>

- With <i><span style="color:#477bbe">configMaps</span></i> and <i><span style="color:#477bbe">secrets</span></i>, instead of specifying a value property, indicate what file the values come from using the <b><span style="color:#d46644">valueFrom</span></b> property
	- Under the <b><span style="color:#d46644">valueFrom</span></b> property, specify either the <b><span style="color:#d46644">configMapKeyRef</span></b> or the <b><span style="color:#d46644">secretKeyRef</span></b> property

![[configure-2.png]]