- Adding and removing additional <b><i><span style="color:#d46644">privileges</span></i></b>, or setting a different user can be configured in <span style="color:#5c7e3e">Kubernetes</span> as well

- Since [[7 - Pods|containers]] are encapsulated in [[7 - Pods|pods]] in <span style="color:#5c7e3e">Kubernetes</span>, you can configure <b><i><span style="color:#d46644">security</span></i></b> at a [[7 - Pods|container]] level or at a [[7 - Pods|pod]] level

- If you configure <b><i><span style="color:#d46644">security</span></i></b> at a [[7 - Pods|pod]] level, the settings will carry over to all the [[7 - Pods|containers]] within the [[7 - Pods|pod]]

- If you configure <b><i><span style="color:#d46644">security</span></i></b> settings on both the [[7 - Pods|pod]] and the [[7 - Pods|container]], the settings on the [[7 - Pods|container]] will override the setting on the [[7 - Pods|pod]]

- To configure <b><i><span style="color:#d46644">security</span></i></b> on a [[7 - Pods|pod]] from within the [[7 - Pods|pod]] definition file, add a field called "<b><i><span style="color:#d46644">securityContext</span></i></b>" under the <span style="color:#5c7e3e">spec</span> section of the [[7 - Pods|pod]]
	- Use the <span style="color:red">runAsUser</span> option to set the <i><span style="color:#477bbe">user</span></i> ID for the [[7 - Pods|pod]]

![[security-1.png]]

- To configure <b><i><span style="color:#d46644">security</span></i></b> on a [[7 - Pods|container]] level from within the [[7 - Pods|pod]] definition file, move the entire <b><i><span style="color:#d46644">securityContext</span></i></b> section under the [[7 - Pods|containers]] section of the file

![[security-2.png]]

- To <b><i><span style="color:#d46644">add/drop capabilities</span></i></b>, use the <span style="color:red">capabilities</span> option under the <span style="color:red">securityContext</span> section and specify which <b><i><span style="color:#d46644">capabilities</span></i></b> to <b><i><span style="color:#d46644">add/drop</span></i></b>
	- *NOTE: <b><i><span style="color:#d46644">Capabilities</span></i></b> are only supported at the [[7 - Pods|container]] level and not the [[7 - Pods|pod]] level

![[security-3.png]]

- If <b><i><span style="color:#d46644">security</span></i></b> is configured at both the [[7 - Pods|container]] and [[7 - Pods|pod]] levels, any [[7 - Pods|containers]] that don't specify the configuration will automatically have that of the [[7 - Pods|pod]]

![[security-4.png]]