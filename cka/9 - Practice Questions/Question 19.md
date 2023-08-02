### Question

[see answer](#answer)

Create a Pod called non-root-pod  with image redis:alpine and the following requirements:

runAsUser: 1000

fsGroup: 2000
























### Answer

[see question](#question)

```shell
$ vim non-root-pod.yaml  
$ kubectl create -f non-root-pod.yaml

apiVersion: v1  
kind: Pod  
metadata:  
  name:  non-root-pod  
spec:  
  securityContext:  
    runAsUser:  1000  
    fsGroup:  2000  
  containers:  
  -  name:  non-root-pod
```

