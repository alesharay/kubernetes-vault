### Question

[see answer](#answer)

Question 5

SIMULATION -

```bash
[student$node-1] $ kubectl config use-context hk8s
```

Task -

Create a new NetworkPolicy named `allow-port-from-namespace` in the existing namespace `fubar`.

Ensure that the new NetworkPolicy allows Pods in namespace internal to connect to port 9000 of Pods in namespace `fubar`.

Further ensure that the new NetworkPolicy:

✑ does not allow access to Pods which don't listen on port 9000

✑ does not allow access from Pods which are not in namespace `internal`










### Answer

[see question](#question)

![[q5a-1.jpg]]

![[q5a-2.jpg]]

























### Answer

[see question](#question)
















### Answer

[see question](#question)


