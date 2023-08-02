### Question

[see answer](#answer)

Create a NetworkPolicy which denies all ingress traffic
























### Answer

[see question](#question)

```shell
$ vim policy.yaml

apiVersion: networking.k8s.io/v1  
kind: NetworkPolicy  
metadata:  
  name: default-deny  
spec:  
  podSelector: {}  
  policyTypes:  
  - Ingress

$ kubectl create -f policy.yaml
```


