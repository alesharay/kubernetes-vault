#flashcards/kubernetes/cka/application-lifecycle-management

- When you have a lot of [[7 - Pods ✅|pod]] definition files, it will become difficult to manage the environment data stored within the query files

- [[3 - Environment Variables ✅|Environment variable]] values can be taken out of the [[7 - Pods ✅|pod]] definition file and managed centrally using <b><span style="color:#d46644">configMaps</span></b>

- <b><span style="color:#d46644">configMaps</span></b> are used to pass configuration data in the form of key-value pairs in <span style="color:#5c7e3e">Kubernetes</span>

- When a <b><span style="color:#d46644">configMap</span></b> is created, inject the <b><span style="color:#d46644">configMap</span></b> into the [[7 - Pods ✅|pod]] instead
	- This way the key-value pairs are available as [[3 - Environment Variables ✅|Environment variables]] for the application hosted inside of the [[7 - Pods ✅|container]] in the [[7 - Pods ✅|pod]]

------------------------------------------------------------------------------------------------------

### Two phases of configuring configMaps

1. Create the <b><span style="color:#d46644">configMap</span></b>
	- There are two ways to create a <b><span style="color:#d46644">configMap</span></b>
		- The imperative way - without using a <b><span style="color:#d46644">configMap</span></b> definition file
			- You can directly specify the key-value pairs in the command line
				- The <span style="color:red">--from-literal</span> option is used to specify the key-value pairs in the command itself
				- To specify multiple key-value pairs, add the <span style="color:red">--from-literal</span> option multiple times
			
				![[configure-2-1.png]] 
				- This method will get complicated when you have too many configuration items
				- Another way to input configuration data is through a file
				- The <span style="color:red">--from-file</span> option is used to specify the file which contains configuration data
	
				![[configure-2-2.png]]

	- The declarative way - by using a <b><span style="color:#d46644">configMap</span></b> definition file
		- With this definition file, we still have the <i><span style="color:#5c7e3e">apiVersion</span></i>, <i><span style="color:#5c7e3e">kind</span></i> and <i><span style="color:#5c7e3e">metadata</span></i> properties; but instead of the <i><span style="color:#5c7e3e">spec</span></i> property, we have the <i><span style="color:#5c7e3e">data</span></i> property
			- Under <i><span style="color:#5c7e3e">data</span></i>, add the configuration data in key-value format
		- You can create as many <b><span style="color:#d46644">configMaps</span></b> in this way as needed for various different purposes
			- Name the config files appropriately as that will be used later while associating it with [[7 - Pods ✅|pods]]

			![[configure-2-3.png]]

- To view <b><span style="color:#d46644">configMaps</span></b>, use the <span style="color:red">kubectl get configmaps</span> command

![[configure-2-4.png]]

- The <span style="color:red">kubectl describe configmaps</span> command lists the configuration data as well under the data section

![[configure-2-5.png]]

1. Inject the <b><span style="color:#d46644">configMap</span></b> in the [[7 - Pods ✅|pod]] definition file

	- Add a property called <b><span style="color:#d46644">envFrom</span></b>, which is a <i><span style="color:#5c7e3e">list</span></i>

	- Each item in the <b><span style="color:#d46644">configMapRef</span></b> <i><span style="color:#5c7e3e">list</span></i> corresponds to a <b><span style="color:#d46644">configMap</span></b> item which specifies the name of the <b><span style="color:#d46644">configMap</span></b>

![[configure-2-6.png]]

------------------------------------------------------------------------------------------------------

### Practice Problems

- How many configMap exist in the default namespace

		kubectl get cm -n default

- Create a new configMap using the following details
	- configMap Name: `webapp-config-map`
	- Data: `APP_COLOR=darkblue`

	`kubectl create cm webapp-config-map --from-literal APP_COLOR=darkblue --dry-run=client -o yaml > webapp-config-map.yaml`

	`kubectl apply -f webapp-config-map.yaml`