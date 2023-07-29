- The concept of service accounts is linked to other security related resources in Kubernetes

- Ie. authentication, authorization, roles, rolebindings, clusterroles, clusterrolebindings, etc

- There are two types of accounts in Kubernetes:

- User
- Service

- User accounts are used by humans

- Ie. Admins accessing the cluster to perform administrative tasks
- Ie. Developers accessing the cluster to deploy applications, etc..

- Service accounts are used by machines

- Service accounts could be accounts used by an application to interact with the Kubernetes cluster

- Ie. A monitoring application like Prometheus uses a service account to poll the Kubernetes API for performance metrics
- Ie. An automated build tool like Jenkins uses a service account to deploy applications on the Kubernetes cluster

- In order for an application to query the Kubernetes API, it has to be authenticated

- For that, you use service accounts

- To create service accounts imperatively, run the command <span style="color:red">kubectl create serviceaccount</span> followed by the account name

![[servicea-1.png]]

- To view service accounts, run the <span style="color:red">kubectl get serviceaccount</span> command

![[servicea-2.png]]

- When a service account is created, it creates a token automatically *

![[servicea-3.png]]

- The service account token is what must be used by the external application while authenticating to the Kubernetes API

- The service account token is stored as a secret object

- If the service account token is not created automatically, one can be created by running the following command:

		kubectl create token <sa name> -n <sa namespace>

- When a service account is created, it first creates a service account object and then generates a token for the service account

- After a service account object is created and a token is generated, a secret is then created to store the token inside, and the secret object is linked to the service account

- To view the service account token, view the secret object first by running the <span style="color:red">kubectl describe serviceaccount</span> command with the name of the service account to find out the name of the secret that was created, then <span style="color:red">kubectl describe secret</span> with the name of the secret to see the encoded token

- Decode the token using base64 decoding

- The service account token can be used as an authentication bearer token when making REST calls to the Kubernetes API

![[servicea-4.png]]

- Using curl, you can provide the bearer token as an authorization header while making the call to the Kubernetes API

![[servicea-5.png]]

- If your application is hosted on the Kubernetes cluster itself, the service account process is different

- If your application is hosted on the Kubernetes cluster, the service account creation and use process is made simple by automatically mounting the service token secret as a volume inside of the pod hosting the application

- This way, the token to access the Kubernetes API is already placed inside of the pod and can be easily read by the application

- For every namespace in Kubernetes, a service account named "default" is automatically created

- Each namespace has it's own "default" service account

- Whenever a pod is created, the default service account and its token are automatically mounted to that pod as a volume mount

- When you look at the details of a pod, you see that a volume is created from the service account secret, which is in fact the secret containing the default token for this service account

![[servicea-6.png]]

- Note the "mounts" section of resources as it points to the locations/paths of the volumes mounted

![[servicea-7.png]]

- If you exec into a pod and list (ls) the files in the location of the mounted volumes, you will see the files that it references in the "volumes" section

![[servicea-8.png]]

- The default service account is very much restricted as it only has permission to run basic Kubernetes API queries

- To use a different service account with more access in your Kubernetes deployed application, modify the pod definition file to include a serviceAccountName field, specifying the name of the new service account

![[servicea-9.png]]

- *Remember: you cannot edit the service account of an existing pod. To use a different service account, you must delete and recreate the pod

- You can edit the service account for a deployment without having to delete and recreate as any changes in the pod spec section will automatically trigger a new rollout

- Thus the deployment will take care of deleting and recreating new pods with the right service accounts

![[servicea-10.png]]

- *Remember: Kubernetes automatically mounts the default service account if you haven't explicitly specified any service account

- You may choose not to mount a service account automatically by setting the "automountServiceAccountToken" property to false in the pod spec section

![[servicea-11.png]]

### Practice Problems

- How many service accounts exist in the default namespace?

		kubectl get serviceaccounts --no-headers | wc -l

- What is the secret token used by the default service account

		kubectl describe serviceaccount default | grep -i token