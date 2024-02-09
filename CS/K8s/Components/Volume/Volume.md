- External storage, outside the [[Pod]]. For persistence. We don't want the data to die in case a [[Pod|pod]] dies
- The Volume can be on the same physical machine as the [[Pod]] or remote, outside of the K8s cluster.
- K8s DO NOT manage data persistence. It's up to the user to setup the appropriate redundancies and backups solutions.
- Think about Volume as an external disk plugged-in the K8s Cluster


```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-db
  labels:
    app: my-db
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-db
  template:
    metadata:
      labels:
        app: my-db
    spec:
      containers:
        - name: mysql
          image: mysql:8.0
          env:
          - name: MYSQL_ROOT_PASSWORD
            value: mypassword
          volumeMounts:
            - name: db-data # This is the name defined under spec>template>spec>volumes>name
              mountPath: "/var/lib/mysql"
      volumes:
        - name: db-data
          persistentVolumeClaim:
            claimName: mysql-data-pvc  # This is the name used by the persistent volume claim
```


If we want to change where the persistent volume is (local, remote, etc), we only need to change the [[Persistent Volume|PV]] and nothing else. The [[Persistent Volume Claim|PVC]] simply says what it needs, regardless of where it's hosted. The [[../Deployment|Deployment]] uses that claim name, regardless of the [[Persistent Volume|PV]] definition.