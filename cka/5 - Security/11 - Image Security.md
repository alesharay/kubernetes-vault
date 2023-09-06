- This lesson will cover image names and configuring pods to use images from secure repositories

- When you use the nginx image, it's actually library/nginx

- When setting the image name, if you don't define a user or account name (the first part of the image name), Kubernetes assumes it to be "library"
	- Library is the name of the default account where Docker's official images are stored

![[images-security-1.png]]

- Docker's official images promote best practices and are maintained by a dedicated team who are responsible for reviewing and publishing the images

- If you have your own Docker account , the pattern would be similar: your-name/image-name

- For images in the Docker registry, they are both stored and pulled from the docker.io DNS

![[images-security-2.png]]

- There are many other popular image registries (ie. Google: gcr.io)

- Public image repositories give anyone access and download privileges

- When you have applications built in-house that shouldn't be made available to the public, hosting an internal private registry may be a good solution

- Many cloud service providers such as AWS, GCP or Azure provide a private registry by default

- On any of the public image registries, you may choose to make your registry private so that it is only accessible using a set of credentials

- From Docker's perspective, to run a container using a private image, you must first login to your private registry using the `docker login` command followed by the private registry name
	- Then input your credentials

![[images-security-3.png]]

![[images-security-4.png]]

- Once successfully logged into the Docker private registry, run the application using the image from the private registry

![[images-security-5.png]]

- In the pod definition file, to use an image from a private registry, we use the full name of the registry along with the image name

![[images-security-6.png]]

- Within Kubernetes, images are pulled by the container runtime  (docker) on the worker nodes

- In order for Kubernetes to get the credentials to access the private registry, thus authenticating the user to the private registry from within Kubernetes, a secret object is created with the credentials in it
	- The secret is of type docker-registry

- Docker-registry is a built-in secret type that was built for storing Docker credentials

- Within the image pull secret, specify the private registry name, the username, the password, and the email address that has access to pull images

![[images-security-7.png]]

- The secret with the Docker private registry credentials is specified in the pod definition file using the "imagePullSecrets" property
	- This a child to the spec property

![[images-security-8.png]]

- When the pod is created with the imagePullSecret details, kubelet (on the worker nodes) uses the secret object to pull images

### Practice Problems

- What secret type must we choose for docker-registry?

		docker-registry

- What is the image used on any application in the cluster?

		kubectl describe pod <pod name> | grep -I image

	(jot down the image name)