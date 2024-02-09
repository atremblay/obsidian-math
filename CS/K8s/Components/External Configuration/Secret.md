---
url:
  - https://kubernetes.io/docs/concepts/configuration/secret/
  - https://kubernetes.io/docs/concepts/configuration/secret/#secret-types
---
- yaml file
- Used to store secret data
- base64 encoded. For example, a user and password would be base64 encoded and copied in this file
- Reference Secret in [[Deployment]]/[[Pod]]


> [!important]
> The Secret component are meant to be encrypted by third-party tools


```yaml
apiVersion: v1
kind: Secret
metadata:
  name: myapp-secret
type: Opaque # https://kubernetes.io/docs/concepts/configuration/secret/#secret-types
data: # Values must be base64-encoded strings
  username: dXNlcm5hbWU= # "username" as base64
  password: cGFzc3dvcmQ= # "password" as base64
```


### How to reference it

Wherever you want to use value from a Secret, use `secretKeyRef` under a `valueFrom` with the name of the Secret (`myapp-secret` defined under `metadata>name` in the Secret yaml config) and the key defined in the Secret yaml config.

```yaml
valueFrom:
  secretKeyRef:
    name: myapp-secret
    key: username
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
        command: ['sh', '-c', "printenv MYSQL_USER"]
        env:
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:  # Reference the Secret
              name: myapp-secret # Secret named myapp-secret
              key: username  # The data defined in the Secret config file
```


> [!important] 
> Secret must be created before being referenced in a component, otherwise it won't be found and the deployment will fail
