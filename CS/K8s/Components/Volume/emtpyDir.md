A local disk to the pod can be configured with a volume type `emptyDir`. This can be useful if two containers inside the same pod want to share data. In this example, two containers share the same volume. The `spec>containers>volumeMounts` specifies where to mount the volume. This can be different for both containers.


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
        command: ['sh', '-c'] 
        args: 
        - while true; do 
            echo "$(date) INFO some app data" >> /var/log/myapp.log;
            sleep 5;
          done
  
        volumeMounts:
        - name: log
          mountPath: /var/log  
 
      - name: log-sidecar
        image: busybox:1.28
        command: ['sh', '-c'] 
        args:
        - tail -f /var/log/myapp.log 
        volumeMounts:
        - name: log
          mountPath: /var/log
  
      volumes: 
      - name: log
        emptyDir: {}
```