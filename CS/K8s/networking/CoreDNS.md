The [[../../networking/DNS|DNS]] server for Kubernetes is CoreDNS. As part of the [[Setup Nodes]], CoreDNS was installed and the pods can be seen with

```
azureuser@master-node:~$ kubectl get pod -n kube-system
NAME                                  READY   STATUS    RESTARTS        AGE
coredns-5dd5756b68-dbqpt              1/1     Running   0               16d # This
coredns-5dd5756b68-wd4jg              1/1     Running   0               16d # And this
etcd-master-node                      1/1     Running   1 (2d ago)      16d
kube-apiserver-master-node            1/1     Running   1 (2d ago)      16d
kube-controller-manager-master-node   1/1     Running   3 (2d ago)      16d
kube-proxy-bw5mq                      1/1     Running   1 (2d ago)      16d
kube-proxy-d48b2                      1/1     Running   0               4h27m
kube-scheduler-master-node            1/1     Running   3 (2d ago)      16d
weave-net-7z5ll                       2/2     Running   0               4h27m
weave-net-zzgzw                       2/2     Running   1 (5h46m ago)   5h46m
```

Every time a new service is added to the cluster, coreDNS will register it so that the [[../Components/Service/Service|Service]]'s name can be used instead of its IP.

```
azureuser@master-node:~$ k exec -it test-nginx-svc -- bash

root@test-nginx-svc:/# cat /etc/resolv.conf
search default.svc.cluster.local svc.cluster.local cluster.local cwkgg322uoiephxawdfaps3ytf.cx.internal.cloudapp.net
nameserver 10.96.0.10
options ndots:5
```

The name `nameserver` and its IP address is the one for the CoreDNS service. It's named `kube-dns`, but it's really CoreDNS. `kube-dns` is an old name/tool that was replaced by CoreDNS.

```
azureuser@master-node:~$ k get svc -n kube-system -o wide
NAME       TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)                  AGE   SELECTOR
kube-dns   ClusterIP   10.96.0.10   <none>        53/UDP,53/TCP,9153/TCP   16d   k8s-app=kube-dns
azureuser@master-node:~$ 
```


> [!important]+ Namespace of the service is important
> The same service name can be used if they are in different namespace. CoreDNS keeps track of the namespace we well.
> 
> | Hostname       | Namespace | Type | Root |
> | -------------- | --------- | ---- | ----|
> | nginx-svc      | default   | svc  | cluster.local |
> | test-nginx-svc | test-ns   | svc  | cluster.local |
> 
> If the service is running in another namespace, simply use the namespace as part of the url, as such
> `<servicename>.<namespace>`
> 
> The Fully Qualified Domain Name is
> `<servicename>.<namespace>.<type>.<Root>`
> e.g.
> `test-nginx-svc.test-ns.svc.cluster.local`
> 
> We don't have to use the FQDN, just enough information to locate a unique entry in the table. CoreDNS will first look in the current namespace. If it does not exists, it requires the \<namespace\>. It will then look further at Type and Root. This is done because of an entry in the `resolv.conf`. It will `search` successively by appending each of the items in the list. Kind of `$PATH` on the command line.
> 
> ```
> root@test-nginx-svc:/# cat /etc/resolv.conf
> search default.svc.cluster.local svc.cluster.local cluster.local cwkgg322uoiephxawdfaps3ytf.cx.internal.cloudapp.net
> nameserver 10.96.0.10
> options ndots:5
> ```
> 

###### Protip: Try the FQDN if you can't reach a service
If the FQDN works, then it probably means there is something wrong in the `search` entry of the `resolv.conf` file.



## How was it setup?

[[tools/kubeadm|kubeadm]] installed it when `kubeadm init`. We can see the config with

```bash
$ kubeadm config print init-defaults
```

```yaml
kubeadm config print init-defaults
apiVersion: kubeadm.k8s.io/v1beta3
bootstrapTokens:
- groups:
  - system:bootstrappers:kubeadm:default-node-token
  token: abcdef.0123456789abcdef
  ttl: 24h0m0s
  usages:
  - signing
  - authentication
kind: InitConfiguration
localAPIEndpoint:
  advertiseAddress: 1.2.3.4
  bindPort: 6443
nodeRegistration:
  criSocket: unix:///var/run/containerd/containerd.sock
  imagePullPolicy: IfNotPresent
  name: node
  taints: null
---
apiServer:
  timeoutForControlPlane: 4m0s
apiVersion: kubeadm.k8s.io/v1beta3
certificatesDir: /etc/kubernetes/pki
clusterName: kubernetes
controllerManager: {}
dns: {}
etcd:
  local:
    dataDir: /var/lib/etcd
imageRepository: registry.k8s.io
kind: ClusterConfiguration
kubernetesVersion: 1.28.0
networking:
  dnsDomain: cluster.local # Here
  serviceSubnet: 10.96.0.0/12 # and here
scheduler: {}
```


