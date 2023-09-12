<i><span style="color:red">Not listed in as a required topic in cert but important to know</span></i>

- [[7 - Pods ✅|Containers]] are not meant to host an OS

- [[7 - Pods ✅|Containers]] are meant to run a specific task or process such as to host an instance of a webserver, application server, database, or even just to carry out some sort of computational task

- A [[7 - Pods ✅|containers]] only lives for as long as the process(es) inside it are alive

- In a <span style="color:#5c7e3e">Dockerfile</span>, the <b><span style="color:#d46644">CMD</span></b> instruction, which stands for <i><span style="color:#d46644">command</span></i>, defines the program that will be run inside of the [[7 - Pods ✅|containers]] when it starts

![[commands-1.png]]

- By default, <span style="color:#5c7e3e">Docker</span> does not attach a terminal to a [[7 - Pods ✅|containers]] when it is run

- In <span style="color:#5c7e3e">Docker</span>, one way to specify a <i><span style="color:#d46644">command</span></i> to run is to append the <i><span style="color:#d46644">command</span></i> to the end of the <span style="color:red">docker run</span> <i><span style="color:#d46644">command</span></i>
	- This way, it overrides the default <i><span style="color:#d46644">command</span></i>

- If the <span style="color:red">docker run</span> <i><span style="color:#d46644">command</span></i> is run with the `sleep 5` <i><span style="color:#d46644">command</span></i> at the end, the [[7 - Pods ✅|containers]] will start, sleep for 5 seconds, then exit

![[commands-2.png]]

- To make a <i><span style="color:#d46644">command</span></i> permanent, you would create your own image from the base image and specify a new <b><span style="color:#d46644">CMD</span></b> in the <span style="color:#5c7e3e">Dockerfile</span>

### Ways to specify a command

- The <i><span style="color:#d46644">command</span></i> can be specified simply in shell format

![[commands-3.png]]

- The <i><span style="color:#d46644">command</span></i> can be specified in a json array

	![[commands-4.png]]
	
	- If using the json array format, the first element in the array should be the executable <i><span style="color:#d46644">command</span></i>

- Using the json array <b><span style="color:#d46644">CMD</span></b> format, do not specify the <i><span style="color:#d46644">command</span></i> and parameters all together. These should be split on spaces

- The <b><span style="color:#d46644">CMD</span></b> option allows <i><span style="color:#d46644">command</span></i>s to be passed in dynamically and that overrides the default <i><span style="color:#d46644">command</span></i>

- To only pass in the parameters to the <span style="color:red">docker run</span> <i><span style="color:#d46644">command</span></i> dynamically (while the executable <i><span style="color:#d46644">command</span></i> is hardcoded), you would use the <b><span style="color:#d46644">ENTRYPOINT</span></b> instruction

- The <b><span style="color:#d46644">ENTRYPOINT</span></b> instruction looks very similar to the <b><span style="color:#d46644">CMD</span></b> instruction.
	- The difference:
		- The <b><span style="color:#d46644">CMD</span></b> instruction requires the <i><span style="color:#d46644">command</span></i> and parameters be passed in as default and when you want to replace, you replace the full <i><span style="color:#d46644">command</span></i> with parameters
			- Replaced
		- The <b><span style="color:#d46644">ENTRYPOINT</span></b> instruction requires the <i><span style="color:#d46644">command</span></i> but not the parameters. The parameters are passed in dynamically
			- Appended

- To configure a default value for the <b><span style="color:#d46644">ENTRYPOINT</span></b> <i><span style="color:#d46644">command</span></i>, you would use both the <b><span style="color:#d46644">ENTRYPOINT</span></b> and <b><span style="color:#d46644">CMD</span></b> instructions together

![[commands-5.png]]

- If you want to modify the <b><span style="color:#d46644">ENTRYPOINT</span></b> instruction at runtime, you can use the <span style="color:red">--entrypoint</span> option with the <span style="color:red">docker run</span> <i><span style="color:#d46644">command</span></i>

![[commands-6.png]]