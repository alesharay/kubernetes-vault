### Question

[see answer](#answer)

Question 4

SIMULATION -

No configuration context change required for this task.

Ensure, however, that you have returned to the base node before starting to work on this task:

```bash
[student@mk8s-master-0] $ exit
```

Task -

First, create a snapshot of the existing etcd instance running at [https://127.0.0.1:2379](https://127.0.0.1:2379), saving the snapshot to `/var/lib/backup/etcd-snapshot.db`.

The following TLS certificates/key are supplied for connecting to the server with etcdctl:

- CA certificate:  `/etc/kubenetes/pki/etcd/ca.crt`
- Client certificate: `/etc/kubernetes/pki/etcd/server.crt`
- Client key: `/etc/kubernetes/pki/etcd/server.key`

Creating a snapshot of the given instance is expected to complete in seconds.

If the operation seems to hang, something's likely wrong with your command. Use CTRL + c to cancel the operation and try again.

Next, restore an existing, previous snapshot located at `$HOME/kubernetes/var/lib/backup/etcd-snapshot-previous.db`.
























### Answer

[see question](#question)

![[q4a.jpg]]

