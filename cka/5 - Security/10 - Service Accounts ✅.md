#flashcards/kubernetes/cka/security

- The concept of <b><i><span style="color:#d46644">service accounts</span></i></b> is linked to other security related resources in <span style="color:#5c7e3e">Kubernetes</span>:
	- authentication
	- authorization
	- roles
	- rolebindings
	- clusterroles
	- clusterrolebindings
	- etc...

- There are two types of accounts in <span style="color:#5c7e3e">Kubernetes</span>:
	- User
	- Service

- <i><span style="color:#477bbe">User</span></i> accounts are used by <u><b><i>humans</i></b></u>
	- Ie. Admins accessing the [[0 - Core Concepts Intro ✅|cluster]] to perform administrative tasks
	- Ie. Developers accessing the [[0 - Core Concepts Intro ✅|cluster]] to deploy applications, etc..

- <b><i><span style="color:#d46644">Service accounts</span></i></b> are used by <u><b><i>machines</i></b></u>

- <b><i><span style="color:#d46644">Service accounts</span></i></b> could be accounts used by an application to interact with the <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]]
	- Ie. A monitoring application like <span style="color:#5c7e3e">Prometheus</span> uses a <b><i><span style="color:#d46644">service account</span></i></b> to poll the <span style="color:#5c7e3e">Kubernetes</span> API for performance metrics
	- Ie. An automated build tool like <span style="color:#5c7e3e">Jenkins</span> uses a <b><i><span style="color:#d46644">service account</span></i></b> to deploy applications on the <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]]

- In order for an application to query the <span style="color:#5c7e3e">Kubernetes</span> API, it has to be [[1 - Authentication ✅|authenticated]]
	- For that, you use <b><i><span style="color:#d46644">service accounts</span></i></b>

- To create <b><i><span style="color:#d46644">service accounts</span></i></b> [[12 - Declarative vs Imperative ✅|imperatively]], run the command <span style="color:red">kubectl create serviceaccount</span> followed by the account name

![[servicea-1.png]]

- To view <b><i><span style="color:#d46644">service accounts</span></i></b>, run the <span style="color:red">kubectl get serviceaccount</span> command

![[servicea-2.png]]

- A <b><i><span style="color:#d46644">service account token</span></i></b> is what must be used by the external application while [[1 - Authentication ✅|authenticating]] to the <span style="color:#5c7e3e">Kubernetes</span> API

- A <b><i><span style="color:#d46644">service account token</span></i></b> can be stored as a [[5 - Secrets ✅|secret]] object

- If the <b><i><span style="color:#d46644">service account token</span></i></b> is not created automatically, one can be created by running the following command:

		kubectl create token <sa name> -n <sa namespace>

- After a <b><i><span style="color:#d46644">service account</span></i></b> object is created, a token can be generated and stored as a [[5 - Secrets ✅|secret]]
	- The [[5 - Secrets ✅|secret]] object can then be linked to the <b><i><span style="color:#d46644">service account</span></i></b>

- To view the <b><i><span style="color:#d46644">service account</span></i></b> token, view the [[5 - Secrets ✅|secret]] object first by running the <span style="color:red">kubectl describe serviceaccount</span> command with the name of the <b><i><span style="color:#d46644">service account</span></i></b> to find out the name of the [[5 - Secrets ✅|secret]] that was created, then <span style="color:red">kubectl describe secret</span> with the name of the [[5 - Secrets ✅|secret]] to see the encoded token
	- Decode the token using <span style="color:#5c7e3e">base64</span> decoding

- The <b><i><span style="color:#d46644">service account</span></i></b> token can be used as an [[1 - Authentication ✅|authentication]] <span style="color:#5c7e3e">bearer token</span> when making <span style="color:#5c7e3e">REST</span> calls to the <span style="color:#5c7e3e">Kubernetes</span> API

![[servicea-4.png]]

- Using <span style="color:#5c7e3e">curl</span>, you can provide the <span style="color:#5c7e3e">bearer token</span> as an [[7 - Authorization ✅|authorization]] header while making the call to the <span style="color:#5c7e3e">Kubernetes</span> API

![[servicea-5.png]]

- If your application is hosted on the <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]] itself, the <b><i><span style="color:#d46644">service account</span></i></b> process is different

- If your application is hosted on the <span style="color:#5c7e3e">Kubernetes</span> [[0 - Core Concepts Intro ✅|cluster]], the <b><i><span style="color:#d46644">service account</span></i></b> creation and use process is made simple by automatically mounting the service token [[5 - Secrets ✅|secret]] as a [[3 - Volumes ✅|volume]] inside of the [[7 - Pods ✅|pod]] hosting the application
	- This way, the token to access the <span style="color:#5c7e3e">Kubernetes</span> API is already placed inside of the [[7 - Pods ✅|pod]] and can be easily read by the application

- For every [[11 - Namespaces ✅|namespace]] in <span style="color:#5c7e3e">Kubernetes</span>, a <b><i><span style="color:#d46644">service account</span></i></b> named "<span style="color:red">default</span>" is automatically created
	- Each [[11 - Namespaces ✅|namespace]] has it's own "<span style="color:red">default</span>" <b><i><span style="color:#d46644">service account</span></i></b>

- Whenever a [[7 - Pods ✅|pod]] is created, the default <b><i><span style="color:#d46644">service account</span></i></b> and its token are automatically mounted to that [[7 - Pods ✅|pod]] as a [[3 - Volumes ✅|volume mount]]

- When you look at the details of a [[7 - Pods ✅|pod]], you see that a [[3 - Volumes ✅|volume]] is created from the <b><i><span style="color:#d46644">service account</span></i></b> [[5 - Secrets ✅|secret]], which is in fact the [[5 - Secrets ✅|secret]] containing the default token for this <b><i><span style="color:#d46644">service account</span></i></b>

![[servicea-6.png]]

- Note the <span style="color:#5c7e3e">mounts</span> section of resources as it points to the locations/paths of the [[3 - Volumes ✅|volumes]] mounted

![[servicea-7.png]]

- If you exec into a [[7 - Pods ✅|pod]] and list (ls) the files in the location of the mounted [[3 - Volumes ✅|volumes]], you will see the files that it references in the <span style="color:#5c7e3e">volumes</span> section

![[servicea-8.png]]

- The default <b><i><span style="color:#d46644">service account</span></i></b> is very much restricted as it only has permission to run basic <span style="color:#5c7e3e">Kubernetes</span> API queries

- To use a different <b><i><span style="color:#d46644">service account</span></i></b> with more access in your <span style="color:#5c7e3e">Kubernetes</span> [[9 - Deployments ✅|deployed]] application, modify the [[7 - Pods ✅|pod]] definition file to include a <span style="color:#5c7e3e">serviceAccountName</span> field, specifying the name of the new <b><i><span style="color:#d46644">service account</span></i></b>

![[servicea-9.png]]

- *Remember: you cannot edit the <b><i><span style="color:#d46644">service account</span></i></b> of an existing [[7 - Pods ✅|pod]]. To use a different <b><i><span style="color:#d46644">service account</span></i></b>, you must delete and recreate the [[7 - Pods ✅|pod]]

- You can edit the <b><i><span style="color:#d46644">service account</span></i></b> for a [[9 - Deployments ✅|deployment]] without having to delete and recreate as any changes in the [[7 - Pods ✅|pod]] <span style="color:#5c7e3e">spec</span> section will automatically trigger a new [[0 - Rolling Updates & Rollbacks ✅|rollout]]
	- Thus the [[9 - Deployments ✅|deployment]] will take care of deleting and recreating new [[7 - Pods ✅|pods]] with the right <b><i><span style="color:#d46644">service accounts</span></i></b>

![[servicea-10.png]]

- *Remember: <span style="color:#5c7e3e">Kubernetes</span> automatically mounts the default <b><i><span style="color:#d46644">service account</span></i></b> if you haven't explicitly specified any <b><i><span style="color:#d46644">service account</span></i></b>

- You may choose not to mount a <b><i><span style="color:#d46644">service account</span></i></b> automatically by setting the <span style="color:#5c7e3e">automountServiceAccountToken</span> property to false in the [[7 - Pods ✅|pod]] <span style="color:#5c7e3e">spec</span> section

![[servicea-11.png]]

### Practice Problems

- How many service accounts exist in the default namespace?

		kubectl get serviceaccounts --no-headers | wc -l

- What is the secret token used by the default service account

		kubectl describe serviceaccount default | grep -i token