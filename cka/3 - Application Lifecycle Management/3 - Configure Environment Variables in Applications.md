- Given a pod definition file, to set an environment variable to use with the Dockerfile, add an env property that consists of an array of dictionaries with properties name and value

- The name is the name of the environment variable made available to the container
- The value is the value of the environment variable made available to the container

![[configure-1.png]]

- Other ways to set the environment variables is to use configMaps and secrets

- With configMaps and secrets, instead of specifying a value property, indicate what file the values come from using the valueFrom property

- Under the valueFrom property, specify either the configMapKeyRef or the secretKeyRef property

![[configure-2.png]]