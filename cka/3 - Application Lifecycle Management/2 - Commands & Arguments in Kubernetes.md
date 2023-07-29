- To append arguments to the <span style="color:red">docker run</span> command, add an args property (in array form) to the containers property of the pod definition file

- To match the json format, put each index on a new line in the pod definition

![[commands-2-1.png]]

- The values in args will override the CMD instruction from the container's Dockerfile

- To override the ENTRYPOINT from the container's Dockerfile, add a command property (in array form) to the containers property of the pod definition file

- To match the json format, put each index on a new line in the pod definition

![[commands-2-2.png]]

- Be sure not to mix the command and args property up. The command property in the pod definition file does not correspond to the CMD instruction in the Dockerfile

- It corresponds with the ENTRYPOINT instruction

Practice Problems

- Create a pod with the ubuntu image to run a container to sleep for 5000 seconds.

kubectl run unbuntu --image=ubuntu --dry-run=client -o yaml --command -- sleep 5000 > ubuntu-sleeper.yaml