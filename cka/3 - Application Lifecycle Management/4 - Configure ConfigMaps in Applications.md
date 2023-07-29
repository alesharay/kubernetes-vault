- When you have a lot of pod definition files, it will become difficult to manage the environment data stored within the query files

- Environment variable values can be taken out of the pod definition file and managed centrally using configMaps

- configMaps are used to pass configuration data in the form of key-value pairs in Kubernetes

- When a configMap is created, inject the configMap into the pod instead

- This way the key-value pairs are available as environment variables for the application hosted inside of the container in the pod

------------------------------------------------------------------------------------------------------

Two phases of configuring config maps

1. Create the configMap

- There are two ways to create a configMap

- The imperative way - without using a configMap definition file

- You can directly specify the key value pairs in the command line

- The <span style="color:red">--from-literal</span> option is used to specify the key-value pairs in the command itself
- To specify multiple key-value pairs, add the <span style="color:red">--from-literal</span> option multiple times

![[configure-2-1.png]]

- This method will get complicated when you have too many configuration items
- Another way to input configuration data is through a file
- The <span style="color:red">--from-file</span> option is used to specify the file which contains configuration data

![[configure-2-2.png]]

- The declarative way - by using a configMap definition file

- With this definition file, we still have the apiVersion, kind and metadata properties; but instead of the spec property, we have the data property

- Under data, add the configuration data in key-value format

- You can create as many configMaps in this way as needed for various different purposes

- Name the config files appropriately as that will be used later while associating it with pods

![[configure-2-3.png]]

- To view configMaps, use the <span style="color:red">kubectl get configmaps</span> command

![[configure-2-4.png]]

- The <span style="color:red">kubectl describe configmaps</span> command lists the configuration data as well under the data section

![[configure-2-5.png]]

1. Inject the configMap in the pod definition file

- Add a property called envFrom, which is a list

- Each item in the configMap list corresponds to a configMap item which specifies the name of the configMap

![[configure-2-6.png]]

------------------------------------------------------------------------------------------------------

Practice Problems

- How many ConfigMaps exist in the default namespace

		kubectl get cm -n default

- Create a new ConfigMap using the following details

- ConfigMap Name: webapp-config-map
- Data: APP_COLOR=darkblue

		kubectl create cm webapp-config-map --from-literal APP_COLOR=darkblue --dry-run=client -o yaml > webapp-config-map.yaml

		kubectl apply -f webapp-config-map.yaml