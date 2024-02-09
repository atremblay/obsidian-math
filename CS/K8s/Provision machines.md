
## Create nodes

Creating a master or a worker node with virtual machines is the same. 
1. Chose/Create a resource group. I created `learning-k8s`
2. Name the VM
3. Use Ubuntu, as usual
4. The master node does not need a whole lot of resources while the worker node may need more. Select the appropriate size
5. Select SSH to login
6. Generate or use an existing key pair
![[images/Pasted image 20231219102703.png]]

### Networking

All the nodes must be on the same network. After creating the master node, I was able to select the same Virtual Network for the worker nodes.

![[images/Pasted image 20231219103009.png]]




## Master node

We are going to install [[tools/kubeadm|kubeadm]] on the master node. The instructions are available [here](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

## swap off
Turn swap off on all the nodes
`sudo swapoff -a`

## Open ports

https://kubernetes.io/docs/reference/networking/ports-and-protocols/

This should probably be done with Ansible. Maybe AKS already set all of this up.
### Control plane

|Protocol|Direction|Port Range|Purpose|Used By|
|---|---|---|---|---|
|TCP|Inbound|6443|Kubernetes API server|All|
|TCP|Inbound|2379-2380|etcd server client API|kube-apiserver, etcd|
|TCP|Inbound|10250|Kubelet API|Self, Control plane|
|TCP|Inbound|10259|kube-scheduler|Self|
|TCP|Inbound|10257|kube-controller-manager|Self|


### Worker node(s)

| Protocol | Direction | Port Range  | Purpose            | Used By             |
| -------- | --------- | ----------- | ------------------ | ------------------- |
| TCP      | Inbound   | 10250       | Kubelet API        | Self, Control plane |
| TCP      | Inbound   | 30000-32767 | NodePort Services† | All                 |


