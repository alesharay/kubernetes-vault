- <b><i><span style="color:#d46644">Storage drivers</span></i></b> help manage <b><i><span style="color:#d46644">storage</span></i></b> on [[11 - Image Security|images]] and [[7 - Pods ✅|containers]]

- If you want to [[4 - Persistent Volumes|persist]] <b><i><span style="color:#d46644">storage</span></i></b>, you must create a [[3 - Volumes|volume]]

- [[3 - Volumes|Volumes]] are not handled by <b><i><span style="color:#d46644">storage drivers</span></i></b>, they are handled by <b><i><span style="color:#d46644">volume driver plugins</span></i></b>

- The default <b><i><span style="color:#d46644">volume driver plugin</span></i></b> is <b><i><span style="color:#d46644">local</span></i></b>

- The <b><i><span style="color:#d46644">local volume driver plugin</span></i></b> helps create a [[3 - Volumes|volume]] on the <span style="color:#5c7e3e">Docker</span> <i><span style="color:#477bbe">host</span></i> and store its data under the <span style="color:red">/var/lib/docker/volumes</span> directory

- Other <b><i><span style="color:#d46644">volume driver plugins</span></i></b> that allow you to create [[3 - Volumes|volumes]] on <span style="color:#5c7e3e">third-party</span> solutions include:
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

- Some <b><i><span style="color:#d46644">volume driver plugins</span></i></b> support different <b><i><span style="color:#d46644">storage</span></i></b> providers
	- Ie. RexRay can be used to provision <b><i><span style="color:#d46644">storage</span></i></b> on AWS EBS, S3, EMC <b><i><span style="color:#d46644">storage</span></i></b> arrays (like iselon and scaleIO), Google persistent disk or Open stack cinder

- When you run a <span style="color:#5c7e3e">Docker</span> [[7 - Pods ✅|container]], you can choose to use a specific <b><i><span style="color:#d46644">volume driver</span></i></b>

![[volume-driver-1.png]]

- Using a specific <b><i><span style="color:#d46644">volume driver</span></i></b> will create a [[7 - Pods ✅|container]] and attach a [[3 - Volumes|volume]] from the chosen <b><i><span style="color:#d46644">driver solution</span></i></b>

- On [[7 - Pods ✅|containers]] with chosen <b><i><span style="color:#d46644">volume drivers</span></i></b>, when the [[7 - Pods ✅|container]] exits, the data is safely stored in the location of that specific <b><i><span style="color:#d46644">driver</span></i></b>