- <i><span style="color:#477bbe">Storage Drivers</span></i> help manage [[0 - Storage in Docker ✅|storage]] on [[11 - Image Security ✅|images]] and [[7 - Pods ✅|containers]]

- If you want to [[4 - Persistent Volumes|persist]] [[0 - Storage in Docker ✅|storage]], you must create a [[3 - Volumes ✅|volume]]

- [[3 - Volumes ✅|Volumes]] are not handled by <i><span style="color:#477bbe">storage drivers</span></i>, they are handled by <b><i><span style="color:#d46644">volume driver plugins</span></i></b>

- The default <b><i><span style="color:#d46644">volume driver plugin</span></i></b> is <b><i><span style="color:#d46644">local</span></i></b>

- The <b><i><span style="color:#d46644">local volume driver plugin</span></i></b> helps create a [[3 - Volumes ✅|volume]] on the <span style="color:#5c7e3e">Docker</span> <i><span style="color:#477bbe">host</span></i> and store its data under the <span style="color:red">/var/lib/docker/volumes</span> directory

- Other <b><i><span style="color:#d46644">volume driver plugins</span></i></b> that allow you to create [[3 - Volumes ✅|volumes]] on <span style="color:#5c7e3e">third-party</span> solutions include:
	- Azure file storage
	- Convoy
	- Digital Ocean block storage
	- Flocker
	- Google computer persistant disk
	- GlusterFS
	- NetApp
	- RexRay
	- Portworx
	- VMWare vSphere  Storage
	- Etc…

- Some <b><i><span style="color:#d46644">volume driver plugins</span></i></b> support different [[0 - Storage in Docker ✅|storage]] providers
	- Ie. RexRay can be used to provision [[0 - Storage in Docker ✅|storage]] on AWS EBS, S3, EMC [[0 - Storage in Docker ✅|storage]] arrays (like iselon and scaleIO), Google persistent disk or Open stack cinder

- When you run a <span style="color:#5c7e3e">Docker</span> [[7 - Pods ✅|container]], you can choose to use a specific <b><i><span style="color:#d46644">volume driver</span></i></b>

![[volume-driver-1.png]]

- Using a specific <b><i><span style="color:#d46644">volume driver</span></i></b> while creating a [[7 - Pods ✅|container]] will attach a [[3 - Volumes ✅|volume]] from the chosen <b><i><span style="color:#d46644">driver solution</span></i></b>

- On [[7 - Pods ✅|containers]] with chosen <b><i><span style="color:#d46644">volume drivers</span></i></b>, when the [[7 - Pods ✅|container]] exits, the data is safely stored in the location of that specific <b><i><span style="color:#d46644">driver</span></i></b>