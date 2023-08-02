### Question

[see answer](#answer)

Create a new ServiceAccount with the name pvviewer.

Grant this ServiceAccount access to list all PersistentVolumes in the cluster by creating an appropriate ClusterRole called pvviewer-role and ClusterRoleBinding called pvviewer-role-binding.

Next, create a pod called pvviewer with the image: redis and serviceaccount: pvviewer in the default namespace.
























### Answer

[see question](#question)

Create Service account

```shell
$ kubectl create serviceaccount pvviewer

Create cluster role

$ kubectl create clusterrole pvviewer-role --verb=list --resource=PersistentVolumes
```

![[q21a-1.png]]

Create cluster role binding

```shell
$ kubectl create clusterrolebinding pvviewer-role-binding --clusterrole=pvviewer-role --serviceaccount=default:pvviewer
```

![[q21a-2.png]]

Verify

```shell
$ kubectl auth can-i list PersistentVolumes –as system:serviceaccount:default:pvviewer
```

![[q21a-3.jpg]]


