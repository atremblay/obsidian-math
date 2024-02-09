The type NodePort for a service creates a service that is reachable outside the cluster.


Quickly create a config file with type NodePort. The port 30000 is from the range specified in the Worker Nodes [[../../Provision machines#Worker node(s)|here]] (30000 - 32767)

```
azureuser@master-node:~$ kubectl create svc nodeport my-svc --tcp="8080:80" --node-port=30000 --dry-run=client  -o yaml
```

```
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: my-svc
  name: my-svc
spec:
  ports:
  - name: 8080-80
    nodePort: 30000
    port: 8080
    protocol: TCP
    targetPort: 80
  selector:
    app: my-svc
  type: NodePort
status:
  loadBalancer: {}
```

The NodePort will open the `nodePort` to the outside. You can reach each nodes directly with its IP address and the port specified here (e.g. 30000)


![[../../images/Pasted image 20240109084302.png|500]]


So when a request comes in (directly to a Node), it then forwards it to the internal Service at port `port` and forwards it to `targetPort`
![[../../images/Pasted image 20240109084216.png|500]]
