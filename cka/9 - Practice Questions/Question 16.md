### Question

[see answer](#answer)

Question 16 ( Topic 1 )

SIMULATION -

[student$node-1] $ kubectl config use-context k3d-ik8s

Task -

Create a new PersistentVolumeClaim:

✑ Name: pv-volume

✑ Class: csi-hostpath-sc

✑ Capacity: 10Mi

Create a new Pod which mounts the PersistentVolumeClaim as a volume:

✑ Name: web-server

✑ Image: nginx

✑ Mount path: /usr/share/nginx/html

Configure the new Pod to have ReadWriteOnce access on the volume.

Finally, using kubectl edit or kubectl patch expand the PersistentVolumeClaim to a capacity of 70Mi and record that change.
























### Answer

[see question](#question)

![[q16a-1.jpg]]

![[q16a-2.jpg]]

![[q16a-3.jpg]]

![[q16a-4.jpg]]

![[q16a-5.jpg]]

![[q16a-6.jpg]]

![[q16a-7.jpg]]


