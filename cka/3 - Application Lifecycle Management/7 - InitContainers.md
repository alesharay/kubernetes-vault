- In a multi-container pod, each container is expected to run a process that stays alive as long as the pod's lifecycle

- If any of them fail, the pod restarts

- In situations where you want to run a process to completion in a container, this is where initContainers come in

- An initContainer is configured in pods like all other containers, except that it is specified inside of an initContainers section

![[initc-1.png]]

- When a pod is first created, the initContainer is run, and the process in the initContainer must run to a completion before the real container hosting the application starts

- Similar to common containers, you can also configure multiple initContainers

- In this case, each initContainer is run one at a time in sequential order

- If any of the initContainers fail to complete, Kubernetes restarts the pod repeatedly until the initContainers succeed