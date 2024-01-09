- <b><i><span style="color:#d46644">IP Address Management</span></i></b> (<b><i><span style="color:#d46644">IPAM</span></i></b>) describes how the [[0.1 - Switching, Routing, Gateways CNI in Kubernetes ✅|virtual bridge networks]] in the [[0 - Core Concepts Intro ✅|nodes]] are assigned an <span style="color:#5c7e3e">IP subnet</span>, how the [[7 - Pods ✅|pods]] are assigned <span style="color:#5c7e3e">IP addresses</span>, and who is responsible for ensuring there are no duplicates assigned

- Per <b><i><span style="color:#d46644">CNI</span></i></b>, it is the responsibility of the <b><i><span style="color:#d46644">CNI plugin</span></i></b> (the <b><i><span style="color:#d46644">network</span></i></b> solution provider) to take care of assigning <span style="color:#5c7e3e">IP address</span> to the containers

- <span style="color:#5c7e3e">Kubernetes</span> doesn't care how the <span style="color:#5c7e3e">IP addresses</span> are managed, it just requires that there are no duplicates and that they are managed properly

- An easy way to manage <span style="color:#5c7e3e">IP addresses</span> is to store the list of <span style="color:#5c7e3e">IP addresses</span> in a file and make sure we have the necessary code in the script to manage the file properly

![[ipam-weave-1.png]]

- The <span style="color:#5c7e3e">IP address</span> file would be placed on each <i><span style="color:#477bbe">host</span></i> and manages the IPs of [[7 - Pods ✅|pods]] on those [[0 - Core Concepts Intro ✅|nodes]]

![[ipam-weave-2.png]]

- Instead of doing any of the above process manually, the <b><i><span style="color:#d46644">CNI</span></i></b> script comes with two built-in <b><i><span style="color:#d46644">plugins</span></i></b> to which these tasks can be outsourced (in this case, it is the <b><i><span style="color:#d46644">host_local plugin</span></i></b>)
	- It is still our responsibility to invoke that <b><i><span style="color:#d46644">plugin</span></i></b>
	- The **script** can also be dynamic to support different kinds of <b><i><span style="color:#d46644">plugins</span></i></b>

- The <b><i><span style="color:#d46644">CNI</span></i></b> configuration file has a section called <b><i><span style="color:#d46644">IPAM</span></i></b> in which we specify the type of <b><i><span style="color:#d46644">plugin</span></i></b>, [[0.1 - Switching, Routing, Gateways CNI in Kubernetes ✅|subnet]], and [[0.1 - Switching, Routing, Gateways CNI in Kubernetes ✅|route]] to be used
	- These details can be read in from the script to invoke the appropriate plugin (instead of hard-coding it)

![[ipam-weave-3.png]]

- Different <b><i><span style="color:#d46644">network</span></i></b> solution providers handle <b><i><span style="color:#d46644">CNI</span></i></b> configuration in different ways (with the standards being the same)

- <b><i><span style="color:#d46644">Weave</span></i></b>, by default, allocates the <span style="color:#5c7e3e">IP range 10.32.0.0/12</span> for the entire <b><i><span style="color:#d46644">network</span></i></b>

- From the available range, the <b><i><span style="color:#d46644">weave</span></i></b> peers will split them equally between each [[0 - Core Concepts Intro ✅|node]] on the <b><i><span style="color:#d46644">network</span></i></b> and [[7 - Pods ✅|pods]] created on those [[0 - Core Concepts Intro ✅|nodes]] will have <span style="color:#5c7e3e">IP addresses</span> in the range specified for their [[0 - Core Concepts Intro ✅|node]]
	- This is by default but can be changed using additional available options

![[ipam-weave-4.png]]
