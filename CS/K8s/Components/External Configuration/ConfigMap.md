---
url:
  - https://kubernetes.io/docs/concepts/configuration/configmap/
---

- yaml file
- External configuration of the application
- Connect it to the [[Pod]]
- If something changes (such as the URL of a database), you can change it in a ConfigMap and not rebuild and redeploy the whole cluster because it was written inside of it.
- Used for non-confidential data only


```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: myapp-config
data:
  # information that we want to make available to the components that will use the ConfigMap
  # add as many as you want
  db_host: mysql-service 
```


### How to reference it

Wherever you want to use value from a ConfigMap, use `configMapKeyRef` under a `valueFrom` with the name of the ConfigMap (`myapp-config` defined under `metadata>name` in the ConfigMap yaml config) and the key defined in the ConfigMap yaml config.

```yaml
valueFrom:
  configMapKeyRef:
    name: <name of the ConfigMap>
    key: <key used in data of the ConfigMap>
```

Example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  labels:
    app: my-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: busybox:1.28
        command: ['sh', '-c', "printenv MYSQL_USER MYSQL_PASSWORD MYSQL_SERVER"]
        env:
        - name: MYSQL_SERVER 
          valueFrom: 
            configMapKeyRef:  # Reference the ConfigMap
              name: myapp-config  # ConfigMap named myapp-config
              key: db_host  # The data defined in the ConfigMap config file

```


> [!important] 
> ConfigMap must be created before being referenced in a component, otherwise it won't be found and the deployment will fail