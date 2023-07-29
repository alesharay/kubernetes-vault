- Adding and removing additional privileges, or setting a different user can be configured in Kubernetes as well

- Since containers are encapsulated in pods in Kubernetes, you can configure security at a container level or at a pod level

- If you configure security at a pod level, the settings will carry over to all the containers within the pod

- If you configure security settings on both the pod and the container, the settings on the container will override the setting on the pod

- To configure security on a pod from within the pod definition file, add a field called "securityContext" under the spec section of the pod

- Use the <span style="color:red">runAsUser</span> option to set the user ID for the pod

![[security-1.png]]

- To configure security on a container level from within the pod definition file, move the entire securityContext section under the containers section of the file

![[security-2.png]]

- To add/drop capabilities, use the <span style="color:red">capabilities</span> option under the <span style="color:red">securityContext</span> section and specify which capabilities to add/drop

- *NOTE: Capabilities are only supported at the container level and not the pod level

![[security-3.png]]

- If security is configured at both the container and pod levels, any containers that don't specify the configuration will automatically have that of the pod

![[security-4.png]]