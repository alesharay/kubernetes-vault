### Question

[see answer](#answer)

Create a new deployment called nginx-deploy, with image nginx:1.16 and 1 replica. Record the version.

Next upgrade the deployment to version 1.17 using rolling update. Make sure that the version upgrade is recorded in the resource annotation.
























### Answer

[see question](#question)

![[q22a-1.png]]

```shell
$ vim nginx-deployment.yaml

$ kubectl apply -f nginx-deployment.yaml --record

$ kubectl get deployment

$ kubectl rollout history deployment nginx-deploy
```

![[q22a-2.png]]

```shell
$ kubectl set image deployment/nginx-deploy nginx=1.17 --record

$ kubectl rollout history deployment nginx-deploy
```

![[q22a-3.png]]

```shell
$ kubectl describe deployment nginx-deploy
```

![[q22a-4.png]]


