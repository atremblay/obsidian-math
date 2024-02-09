---
aliases:
  - PVC
---
A Persistant Volume Claim is a space allocation claimed from the [[Persistent Volume]]. At this point we have no idea how much storage is available, where it is or how many volumes. We are simply making a claim for a certain amount of storage. It's the [[Persistent Volume|PV]]'s job to allocate it at the right place.

- In PVC, we define how much storage, access mode, etc we need
- K8s Control Plane looks for a PV that satisfies the claim's requirements (storage space, access modes, etc)
- When a suitable PV is found, it binds the claim to the volume. **Otherwise it will stay unbound**.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: mysql-data-pvc
spec:
    resources:
        requests:
            storage: 5Gi
    accessModes:
    - ReadWriteMany
```


Considering the basic config definition in [[Persistent Volume]], this is the current status of the PVC.
```
azureuser@master-node:~$ kubectl get pvc
NAME             STATUS   VOLUME    CAPACITY   ACCESS MODES   STORAGECLASS   AGE
mysql-data-pvc   Bound    data-pv   10Gi       RWX                           8s
```

**Note:** The specification doesn't need to match 100%, but is must be the most fitting option. We see here that the capacity here is 10Gi even if we asked for 5Gi. So asking 5Gi twice will not be put on the same volume.