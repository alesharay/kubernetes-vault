### Question

[see answer](#answer)

Create a PersistentVolume with the given specification.

Volume Name: pv-analytics

Storage: 100Mi

Access modes: ReadWriteMany

Host Path: /pv/data-analytics
























### Answer

[see question](#question)

```shell
$ vim pv.yaml

apiVersion: v1  
kind: PersistentVolume  
metadata:  
  name: pv-analytics  
spec:  
  capacity:  
    storage: 100Mi  
  accessModes:  
    - ReadWriteMany  
  hostPath:  
    path:  /pv/data-analytics

$ kubectl create -f pv.yaml  
$ kubectl get pv
```

![[q18a.png]]
