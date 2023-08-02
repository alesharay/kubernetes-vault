### Question

[see answer](#answer)

Question 13

SIMULATION -

[student$node-1] $ kubectl config use-context k3d-k8s

Context -

An existing Pod needs to be integrated into the Kubernetes built-in logging architecture (e.g. kubectl logs). Adding a streaming sidecar container is a good and common way to accomplish this requirement.

Task -

Add a sidecar container named sidecar, using the busybox image, to the existing Pod big-corp-app. The new sidecar container has to run the following command:

/bin/sh -c "tail -n+1 -f /var/log/big-corp-app.log"

Use a Volume, mounted at /var/log, to make the log file big-corp-app.log available to the sidecar container.

Don't modify the specification of the existing container other than adding the required volume mount.
























### Answer

[see question](#question)

![[q13a-1.jpg]]

![[q13a-2.jpg]]

![[q13a-3.jpg]]

