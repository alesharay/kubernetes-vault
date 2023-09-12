- <b><span style="color:#d46644">Secrets</span></b> are used to store sensitive information like passwords or keys

- <b><span style="color:#d46644">Secrets</span></b> are similar to [[4 - ConfigMaps|configMaps]] except that they're stored in an <span style="color:#5c7e3e">encoded</span> or <span style="color:#5c7e3e">hashed</span> format

![[secrets-1.png]]

------------------------------------------------------------------------------------------------------

### Two phases involved when working with secrets

1. Create the <b><span style="color:#d46644">secret</span></b>
	- Similar to [[4 - ConfigMaps|configMaps]], there are two ways to creating a <b><span style="color:#d46644">secret</span></b>
		- <span style="color:#5c7e3e">Imparatively</span>

	![[secrets-2.png]]

		- <span style="color:#5c7e3e">Declaratively</span>
			- Be sure to store the values for the <b><span style="color:#d46644">secret</span></b> in encoded or hashed format

![[secrets-3.png]]

- To convert the data to <span style="color:#5c7e3e">encoded</span> or <span style="color:#5c7e3e">hashed</span> format, use <span style="color:#5c7e3e">base64 encoding</span>
	- Be sure to add the <span style="color:red">-n</span> option to remove any newline characters

![[secrets-4.png]]

- To view the original values of the <span style="color:#5c7e3e">base64 encoded</span> <b><span style="color:#d46644">secret</span></b>, use the same <span style="color:#5c7e3e">base64</span> command with the <span style="color:red">--decode ( -d )</span> command

![[secrets-7.png]]

1. Inject the <b><span style="color:#d46644">secret</span></b> into the [[7 - Pods ✅|pod]] definition file

	- To inject an [[3 - Environment Variables|environment variable]] into the [[7 - Pods ✅|pod]] definition file, add the <b><span style="color:#d46644">envFrom</span></b> property

	- The <b><span style="color:#d46644">envFrom</span></b> variable is a list and can hold as many [[3 - Environment Variables|environment variables]] as required

![[secrets-6.png]]

- Another way to inject <b><span style="color:#d46644">secrets</span></b> into a [[7 - Pods ✅|pod]] definition file is as files in a [[3 - Volumes|volume]] if you were to mount the <b><span style="color:#d46644">secret</span></b> as a [[3 - Volumes|volume]] in your [[7 - Pods ✅|pod]]
	- Each attribute in the <b><span style="color:#d46644">secret</span></b> is created as a file with the value of the <b><span style="color:#d46644">secret</span></b> as its content

![[secrets-7.png]]

------------------------------------------------------------------------------------------------------

- To view <b><span style="color:#d46644">secrets</span></b>, run the <span style="color:red">kubectl get secrets</span> command

- To view more information regarding the <b><span style="color:#d46644">secret</span></b>, use the <span style="color:red">kubectl describe secrets</span> command

### Practice Problems

- Create a new secret named `db-secret` with the given data:
	- Secret Name: `db-secret`
	- Secret 1: `DB_Host=sql01`
	- Secret 2: `DB_User=root`
	- Secret 3: `DB_Password=password123`

	`kubectl create secret generic db-secret --from-literal=DB_Host=sql01 --from-literal=DB_User=root --from-literal=DB_Password=password123 --dry-run=client -o yaml > db-secret.yaml`