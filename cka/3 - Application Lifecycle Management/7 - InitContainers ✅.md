#flashcards/kubernetes/cka/application-lifecycle-management

- In a [[6 - Multi-Container Pods ✅|multi-container pod]], each [[7 - Pods ✅|container]] is expected to run a process that stays alive as long as the [[7 - Pods ✅|pod's]] lifecycle
	- If any of them fail, the [[7 - Pods ✅|pod]] restarts

- In situations where you want to run a process to completion in a [[7 - Pods ✅|container]], this is where <b><span style="color:#d46644">initContainers</span></b> come in

- An <b><span style="color:#d46644">initContainer</span></b> is configured in [[7 - Pods ✅|pods]] like all other [[7 - Pods ✅|containers]], except that it is specified inside of an <b><span style="color:#d46644">initContainers</span></b> section

![[initc-1.png]]

- When a [[7 - Pods ✅|pod]] is first created, the <b><span style="color:#d46644">initContainer</span></b> is run, and the process in the <b><span style="color:#d46644">initContainer</span></b> must run to completion before the real [[7 - Pods ✅|container]] hosting the application starts

- Similar to common [[7 - Pods ✅|containers]], you can also configure multiple <b><span style="color:#d46644">initContainers</span></b>
	- In this case, each <b><span style="color:#d46644">initContainer</span></b> is run one at a time in sequential order

- If any of the <b><span style="color:#d46644">initContainers</span></b> fail to complete, <span style="color:#5c7e3e">Kubernetes</span> restarts the [[7 - Pods ✅|pod]] repeatedly until the <b><span style="color:#d46644">initContainers</span></b> succeed