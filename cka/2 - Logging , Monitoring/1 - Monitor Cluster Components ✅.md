### Logging in Docker

- To see <span style="color:#5c7e3e">docker</span> logs, use the <span style="color:red">docker logs -f</span> command followed by the container ID
	- The <span style="color:red">-f</span> option helps view the live log trail

### Logging in Kubernetes

- To see object logs, use the <span style="color:red">kubectl logs -f</span> command followed by the object name
	- The <span style="color:red">-f</span> option still shows a stream of the logs live

- If there are [[7 - Pods ✅|multiple containers]] within a [[7 - Pods ✅|pods]], you must specify the name of the [[7 - Pods ✅|containers]] explicitly in the logs command, using the <span style="color:red">-c</span> option, to see the logs for that container
	- Otherwise the command will fail if you fail to specify a [[7 - Pods ✅|containers]] name