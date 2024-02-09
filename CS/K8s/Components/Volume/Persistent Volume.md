---
url:
  - https://kubernetes.io/docs/concepts/storage/persistent-volumes/
aliases:
  - PV
---
Persistent Volume is a volume space reserved either on a remote machine or a local one.

Minimal working config for a local storage [[hostPath]]

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
    name: data-pv
spec:
    hostPath: # This is the type of persistent volume. The spec below will be specific to each types.
        path: "/mnt/data"
    capacity: # Common to all types https://kubernetes.io/docs/concepts/storage/persistent-volumes/#types-of-persistent-volumes
        storage: 10Gi
    accessModes: # Common to all types https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
    - ReadWriteMany
```

The STATUS available means that nothing is using it at the moment.

```
azureuser@master-node:~$ kubectl get pv
NAME      CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
data-pv   10Gi       RWX            Retain           Available                                   7s
```

That space can now be claimed by a [[Persistent Volume Claim]]. Once it's claimed, it will look like this

```
azureuser@master-node:~$ kubectl get pv
NAME      CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                    STORAGECLASS   REASON   AGE
data-pv   10Gi       RWX            Retain           Bound    default/mysql-data-pvc                           12m
```