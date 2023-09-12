### Pod Lifecycle Stages

* A [[7 - Pods ✅|pod]] has a [[7 - Pods ✅|pod]] status and some conditions

![[readiness-1.png]]

* The [[7 - Pods ✅|pod]] status tells us where the pod is in its lifecycle

* When a [[7 - Pods ✅|pod]] is first created, it is in a pending state.
	* This is when the **scheduler** determines where to place the **pod**
	* If the **scheduler** cannot find a **node** to place the **pod** it will remain in a *pending* state

![[readiness-2.png]]

* Once the **pod** is *scheduled*, it goes into a *containerCreating* status
	* This is where the **images** required for the application are pulled and the **container** starts

* Once all of the **containers** in a **pod** starts, it goes into a *running* state
	* This is where **pod** will remain until the program completes successfully, or is terminated

![[readiness-3.png]]

* **Conditions** compliment **pod status** as it is an *array* of *true* or *false* values that tell us the state of the **pod**d

![[readiness-4.png]]

* When a **pod** is *scheduled* on a **node**, the **podScheduled** condition is set to *true*

![[readiness-5.png]]

* When the **pod** is initialized, the **initialized** condition is set to true

![[readiness-6.png]]

* When all of the **containers** in a **pod** are ready, the **containersReady** condition is set to true

![[readiness-7.png]]

* When all of the previous **conditions** are true, the **ready condition** is set to true as the **pod** is now ready

![[readiness-8.png]]

* The **ready condition** indicates that the application inside of the **pod** is running and ready to accept user traffic

* The **containers** inside of a **pod** can have many different types of applications in them
	* A *script* which may take just a few milliseconds to warm up
	* A *DB server* which can take a few seconds to load
	* A *web server* which can take a few minutes to load
	* etc...

* During the wait period of the **containers** actually being ready to accept traffic, the **ready condition** will falsely show true
	* This is because even though the containers are running, they may not actually be ready to accept traffic

* In a scenario where you create a **pod** and expose to external users using a **service**, the **service** will route traffic to the **pod** immediately
	* By default, Kubernetes assumes that as soon as a **container** is running, it is ready to start serving traffic so it sets the value of the **ready condition** for each container to true
	* **container** took longer to get ready, the **service** is unaware of this through is if the **container** is already prepared to accept traffic, which causes users to hit a **pod** that isn't yet running a live application

![[readiness-9.png]]

* What's needed is a way to tie the ready status to the actual state of the application

### Readiness Probe

* There are different ways to define if the application inside of a **container** is actually ready
	* You can uses different kinds of tests (probes)

* If your container is running a web-application, you can run an HTTP test to hit an endpoint that waits for a response

![[readiness-10.png]]

* If your container is running a database, you can run a TCP socket test to check if a particular port is open and listening

![[readiness-11.png]]

* You can also execute a command or run a script using the Exec test to check if the command/scripts exits successfully indicating that the application is ready

![[readiness-12.png]]

* In the **spec.containers** field of the **pod** definition (or template in a **deployment** definition), add an object called **readinessProbe**
	* Depending on the type of test you want to run, the inner properties will be different

![[readiness-13.png]]

* For the HTTP Get test, specify the path (endpoint) and ports to hit (likely the ready api)

![[readiness-14.png]]

* For the TCP test, specify the port inside of the **tcpSocket** object

![[readiness-15.png]]

* For executing a command, specify the **exec** object with the command property, then add the commands in array format

![[readiness-16.png]]

* Now, Kubernetes will not immediately set the **ready condition** in the **pod** to *true*
	* Instead, it will perform a test to see if the api responds positively
	* Until then, the servers will not send any traffic to the **pod** as it sees that the **pod** is not ready

* If you know how much time your app will take to warm up, you can add an *initial delay* to the probe with the **initialDelaySeconds** property

* To specify how often the probe should run, use the **periodSeconds** option

* By default, if the application is not ready after 3 attempts, the probe will stop and set the **ready** condition to false. 
	* To make more attempts, set the **failureThreshold** property

![[readiness-17.png]]

* d
