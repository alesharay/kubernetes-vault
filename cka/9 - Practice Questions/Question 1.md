### Question

[see answer](#answer)

Question 1

SIMULATION -

Set configuration context

```bash
[student$node-1] $ kubectl config use-context k8s
```

Context -

You have been asked to create a new ClusterRole for a deployment pipeline and bind it to a specific ServiceAccount scoped to a specific namespace.

Task -

Create a new ClusterRole named `deployment-clusterrole`, which only allows to create the following resource types:

✑ Deployment

✑ StatefulSet

✑ DaemonSet

Create a new ServiceAccount named `cicd-token` in the existing namespace `app-team1`.

Bind the new Clusterrole `deployment-clusterrole` to the new ServiceAccount `cicd-token`, limited to the namespace `app-team1`.
























### Answer

[see question](#question)

![[q1a.jpg]]

