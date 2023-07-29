- Secrets are used to store sensitive information like passwords or keys

- Secrets are similar to configMaps except that they're stored in an encoded or hashed format

![[secrets-1.png]]

------------------------------------------------------------------------------------------------------

### Two phases involved when working with secrets

1. Create the secret

- Similar to configMaps, there are two ways to creating a secret

- Imperatively

![[secrets-2.png]]

- Declaratively

- Be sure to store the values for the secret in encoded or hashed format

![[secrets-3.png]]

- To convert the data to encoded or hashed format, use base64 encoding

- Be sure to add the <span style="color:red">-n</span> option to remove any newline characters

![[secrets-4.png]]

- To view the original values of the base64 encoded secret, use the same base64 command with the <span style="color:red">--decode ( -d )</span> command

![[secrets-7.png]]

1. Inject the secret into the pod definition file

- To inject an environment variable into the pod definition file, add the envFrom property

- The envFrom variable is a list and can hold as many environment variables as required

![[secrets-6.png]]

- Another way to inject secrets into a pod definition file is as files in a volume if you were to mount the secret as a volume in your pod

- Each attribute in the secret is created as a file with the value of the secret as its content

![[secrets-7.png]]

------------------------------------------------------------------------------------------------------

- To view secrets, run the <span style="color:red">kubectl get secrets</span> command

- To view more information regarding the secret, use the <span style="color:red">kubectl describe secrets</span> command

Practice Problems

- Create a new secret named "db-secret" with the given data:

- Secret Name: db-secret
- Secret 1: DB_Host=sql01
- Secret 2: DB_User=root
- Secret 3: DB_Password=password123

		kubectl create secret generic db-secret --from-literal=DB_Host=sql01 --from-literal=DB_User=root --from-literal=DB_Password=password123 --dry-run=client -o yaml > db-secret.yaml