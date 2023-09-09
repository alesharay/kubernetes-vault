- A <span style="color:#5c7e3e">Docker</span> <i><span style="color:#477bbe">host</span></i> has a set of its own processes running on it (i.e. a number of operating system processes, the <span style="color:#5c7e3e">Docker daemon</span> itself, the <span style="color:#5c7e3e">SSH</span> server, etc…)

![[psd-1.png]]

- *Remember: unlike virtual machines, [[7 - Pods|containers]] are not completely isolated from their <i><span style="color:#477bbe">host</span></i>

![[psd-2.png]]

- [[7 - Pods|Containers]] and their <i><span style="color:#477bbe">host</span></i> share the same <span style="color:#5c7e3e">kernel</span>

- [[7 - Pods|Containers]] are isolated using [[11 - Namespaces|namespaces]] in <span style="color:#5c7e3e">Linux</span>

- The <i><span style="color:#477bbe">host</span></i> has a [[11 - Namespaces|namespace]] and the [[7 - Pods|containers]] have their own [[11 - Namespaces|namespace]]

![[psd-3.png]]

- All processes run by a [[7 - Pods|container]] are in fact run on the <i><span style="color:#477bbe">host</span></i> itself but in their own [[11 - Namespaces|namespace]]
	- As far as the <span style="color:#5c7e3e">Docker</span> [[7 - Pods|container]] is concerned, it is in its own [[11 - Namespaces|namespace]] and it can see its own processes only
	- It cannot see anything outside of it or in any other [[11 - Namespaces|namespace]]

- When you list processes within the <span style="color:#5c7e3e">Docker</span> [[7 - Pods|container]], you see the processes with their process IDs

![[psd-4.png]]

- For the <span style="color:#5c7e3e">Docker</span> <i><span style="color:#477bbe">host</span></i>, all processes of its own, as well as those in the child [[11 - Namespaces|namespaces]] are visible as just another process in the system

- When you list the processes on the <i><span style="color:#477bbe">host</span></i>, you see a list of processes and there IDs
	- These will show the same processes as the [[7 - Pods|container]] (including more) but with different process IDs

![[psd-5.png]]

- Processes can have different process IDs in different [[11 - Namespaces|namespaces]] and this is how <span style="color:#5c7e3e">Docker</span> isolates [[7 - Pods|containers]] within a system

- The <span style="color:#5c7e3e">Docker</span> <i><span style="color:#477bbe">host</span></i> has a set of <i><span style="color:#477bbe">users</span></i>: a <i><span style="color:#477bbe">root user</span></i> and a slew of <i><span style="color:#477bbe">non-root users</span></i>

- By default, <span style="color:#5c7e3e">Docker</span> runs processes within [[7 - Pods|containers]] as the <i><span style="color:#477bbe">root user</span></i>

- Both in the [[7 - Pods|container]] and outside of the [[7 - Pods|container]] as the <i><span style="color:#477bbe">host</span></i>, the process is run as the <i><span style="color:#477bbe">root user</span></i>

- If you don't want processes within the [[7 - Pods|container]] to run as the <i><span style="color:#477bbe">root user</span></i>, you may set the <i><span style="color:#477bbe">user</span></i> using the <span style="color:red">kubectl --user</span> flag within the <span style="color:#5c7e3e">Docker</span> run command
	- You will see that the process now runs with the new <i><span style="color:#477bbe">user</span></i> ID

![[psd-6.png]]

- Another way to enforce <i><span style="color:#477bbe">user</span></i> <b><i><span style="color:#d46644">security</span></i></b> in [[7 - Pods|containers]] is to define the <i><span style="color:#477bbe">user</span></i> in the [[11 - Image Security|images]] themselves at the time of creation

- To define the <i><span style="color:#477bbe">user</span></i> in the [[11 - Image Security|image]], use the <i><span style="color:#477bbe">USER</span></i> instruction in the <span style="color:#5c7e3e">Dockerfile</span> then give it the <i><span style="color:#477bbe">user</span></i> ID, then build the custom [[11 - Image Security|image]]

![[psd-7.png]]

- When the <i><span style="color:#477bbe">user</span></i> is specified within the [[11 - Image Security|image]], you can run the [[7 - Pods|container]] without specifying the <i><span style="color:#477bbe">user</span></i> and when you output the processes, you will see the <i><span style="color:#477bbe">user</span></i> is the <i><span style="color:#477bbe">user</span></i> specified at creation time

![[psd-8.png]]

- <span style="color:#5c7e3e">Docker</span> implements a set of <b><i><span style="color:#d46644">security</span></i></b> features that limit the abilities of the <i><span style="color:#477bbe">root user</span></i> within the [[7 - Pods|container]]
	- This means that the <i><span style="color:#477bbe">root user</span></i> within the [[7 - Pods|container]] isn't really like the <i><span style="color:#477bbe">root user</span></i> on the <i><span style="color:#477bbe">host</span></i>

- *Remember: the <i><span style="color:#477bbe">root user</span></i> is usually the most powerful <i><span style="color:#477bbe">user</span></i> on a system and can do literally anything
	- And so can a process run by the <i><span style="color:#477bbe">root user</span></i>

![[psd-9.png]]

- On a <span style="color:#5c7e3e">Linux</span> operating system, you can see a full list of <i><span style="color:#477bbe">root user</span></i> capabilities at <span style="color:red">/usr/include/linux/capability.h</span>

- By default, <span style="color:#5c7e3e">Docker</span> runs a [[7 - Pods|container]] with a limited set of capabilities
	- Thus the processes run within the [[7 - Pods|container]] do not have the privileges to do certain things that require full access

![[psd-10.png]]

- If you wish to override the default behavior of the <span style="color:#5c7e3e">Docker</span> [[7 - Pods|container]] and provide additional privileges than what is available, then use the "<span style="color:red">cap-add</span>" option in the <span style="color:red">docker run</span> command

![[psd-11.png]]

- Similar to adding privileges to a [[7 - Pods|container]] <i><span style="color:#477bbe">user</span></i>, you drop privileges using the "<span style="color:red">cap drop</span>" option with the <span style="color:red">docker run</span> command

![[psd-12.png]]

- If you wish to run the [[7 - Pods|container]] with all privileges available, use the "<span style="color:red">--privileged</span>" flag with the <span style="color:red">docker run</span> command