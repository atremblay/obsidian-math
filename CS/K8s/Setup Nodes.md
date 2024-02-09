---
url:
  - https://v1-28.docs.kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
  - https://v1-28.docs.kubernetes.io/docs/setup/production-environment/container-runtimes/
---

### Install Container Runtime (containerd)

[[Architecture/Worker|Worker]] and [[Architecture/Master|Master]] nodes need to have the runtime installed. This is a manual process described in the URL above, but the gist of it is:


```shell
#!/bin/bash

# Install and configure prerequisites
## load the necessary modules for Containerd
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter

# sysctl params required by setup, params persist across reboots
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

# Apply sysctl params without reboot
sudo sysctl --system

# Install containerd
sudo apt-get update
sudo apt-get -y install containerd

# Configure containerd with defaults and restart with this config
sudo mkdir -p /etc/containerd
containerd config default | sudo tee /etc/containerd/config.toml
sudo sed -i 's/SystemdCgroup \= false/SystemdCgroup \= true/g' /etc/containerd/config.toml
sudo systemctl restart containerd
```

Again this would make more sense to run with Ansible. I am guessing that AKS sets all of that up for us.



### Install kubeadm, kubelet and kubectl

This needs to run on all nodes (worker and master)

```shell
sudo apt-get update
# apt-transport-https may be a dummy package; if so, you can skip that package
sudo apt-get install -y apt-transport-https ca-certificates curl gpg

# The next commands require this destination to exists
sudo mkdir /etc/apt/keyrings

# Download the public signing key for the Kubernetes package repositories. The same signing key is used for all repositories so you can disregard the version in the URL:
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

# Add the appropriate Kubernetes `apt` repository. Please note that this repository have packages only for Kubernetes 1.28; for other Kubernetes minor versions, you need to change the Kubernetes minor version in the URL to match your desired minor version (you should also check that you are reading the documentation for the version of Kubernetes that you plan to install).

# This overwrites any existing configuration in /etc/apt/sources.list.d/kubernetes.list
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

# Update the `apt` package index, install kubelet, kubeadm and kubectl, and pin their version:
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

### Initialize Cluster with kubeadm
This needs to run on all master and worker nodes.

`sudo kubeadm init`

This has a few phases: 
- **preflight**:  checks to validate the system state before making changes
- **certs**: Generates a self-signed CA to set up identities for each component in the cluster
- **kubeconfig**: Writer kubeconfig in `/etc/kubernetes` for [[tools/kubelet|kubelet]], controller-manager and scheduler to use to connect to the API server as well as kubeconfig file for administration
- **kubelet-start**: writes [[tools/kubelet|kubelet]] settings and (re)start the kubelet
- **control-plane**: Generates static [[Components/Pod|Pod]] manifests for the API server, controller-manager and scheduler into `/etc/kubernetes/manifests` directory. [[tools/kubelet|kubelet]] watches this directory for [[Components/Pod|Pod]] to create on startup
    - You can look into `/etc/kubernetes/manifests` to see (and troubleshoot) how all the [[Architecture/Master#Processes|processes]] (e.g. etcd) are setup.
- **addons**: Installs a DNS server (CoreDNS) and the kube-proxy addon components via the API server

#### Running pods in the Master Node (namespace kube-system)

This installed all the [[Architecture/Master#Processes|processes for the master node]]. We can list them by listing all the pods in the [[Namespaces|namespace]] *kube-system*.

```shell
>>> sudo kubectl get pod --kubeconfig /etc/kubernetes/admin.conf -n kube-system
NAME                                  READY   STATUS    RESTARTS   AGE
coredns-5dd5756b68-dbqpt              0/1     Pending   0          4h33m
coredns-5dd5756b68-wd4jg              0/1     Pending   0          4h33m
etcd-master-node                      1/1     Running   0          4h34m
kube-apiserver-master-node            1/1     Running   0          4h34m
kube-controller-manager-master-node   1/1     Running   0          4h34m
kube-proxy-bw5mq                      1/1     Running   0          4h33m
kube-scheduler-master-node            1/1     Running   0          4h34m
```


### Connect to the cluster (kubeconfig & kubectl)

In a previous tutorial we used `kubectl` after setuping [[tools/minikube|minikube]]. That (probably) took care of telling [[tools/kubectl|kubectl]] where the API server of the [[Architecture/Master|master node]] is. So a barebone `kubectl get node` would get us nothing.

We have to tell it what config to use (i.e. how to connect to the [[Architecture/Master#1. API server|API Server]])
```shell
>>> sudo kubectl get node --kubeconfig /etc/kubernetes/admin.conf
NAME          STATUS     ROLES           AGE   VERSION
master-node   NotReady   control-plane   46m   v1.28.5
```

[[tools/kubectl|kubectl]] precedence of kubeconfig file used:
1. File passed with `--kubeconfig` flag
2. `KUBECONFIG` environment variable
3. File located in `$HOME/.kube/config`

Third option is the easiest and the best. But copying the config file on each machine will probably be done with Ansible.


> [!important] 
> That config file just has to be where `kubectl` looks by default, which is `~/.kube`. Copy `/etc/kubernetes/admin.conf` there, change to permission so the current user owns it et voilà.
> ```shell
> mkdir -p $HOME/.kube  
> sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config  
> sudo chown $(id -u):$(id -g) $HOME/.kube/config
> ```
> Permissions are changed and the file can be used
> ```shell
> azureuser@master-node:~$ ls -l ~/.kube/config
> -rw------- 1 azureuser azureuser 5644 Jan  3 20:14 /home/azureuser/.kube/config
> ```



#### admin.conf example

`clusters>cluster>server` is where the [[Architecture/Master#1. API server|API server]] runs. Refer to [[Provision machines#Control plane|opened ports]] for the master node to see where 6443 comes from.

`contexts>context>cluster` refers to `cluters>cluster>name`. This tells [[tools/kubectl|kubectl]] what is the current cluster to use. 

```yaml
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURCVENDQWUyZ0F3SUJBZ0lJV3BSUGM2L1JxQzR3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TXpFeU1qQXhORE14TURaYUZ3MHpNekV5TVRjeE5ETTJNRFphTUJVeApFekFSQmdOVkJBTVRDbXQxWW1WeWJtVjBaWE13Z2dFaU1BMEdDU3FHU0liM0RRRUJBUVVBQTRJQkR3QXdnZ0VLCkFvSUJBUURVS0RuUkZybzUzeWg1c0c1M2NXcmVTbFYvdnhtdjdmSEFDOTNWaXhWQW9NSmJNMGxYYzdFQTNEUVkKdXhiajR1YmJQkFRQnVkKZEdYSThabU1YNCtEMjhqUmYxQTlSYXEyMGpVTFZoTlFFQkdWMlJVVzRSditteG03OEdEc1FESU56SWJMeVNuawpWdUwzVCtBa1ZDV1JNRGVpM2hpLzRBeDZOKy8rYk9Ec205K1VOZVI4SWVQcmQxaysrSkNuKzNFNTJBZFQxbHh5CjNaWUNBK2U2anQvOHdGb1lPQ1VKaCtKZzgxU2F0YWNZVnd1L2s2THVlQ2FFQjJCK3NscWdjMW5FV1UvelE0eXEKMUxNNEhrM2trU0FqCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
    server: https://10.1.0.4:6443 # The API server (Master Node) runs here
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: kubernetes-admin
  name: kubernetes-admin@kubernetes
current-context: kubernetes-admin@kubernetes
kind: Config
preferences: {}
users:
- name: kubernetes-admin
  user:
    client-certificate-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURJVENDQWdtZ0F3SUJBZ0lJTVQ3VjFKT1VXN2d3RFFZSktvWklodmNOQVFFTEJRQXdiUXRqRlhoQXNId25DNVBsdVA3MlA0ajdEcUU5QVJpR0ZKCm0wZExLc2h1bXI2OWRMNTVBK3FrdDNrYUZ4Wkl3dysxQnk5R0Z5SFZERnBqT25wY0pac2tCYjlXdHo5a3JLUzQKekhjVGxBRGYxUzhHNC85Y1ZlRXYwWnYrby9sdXJGakdTWk1rR2xDQTBXdDllMDR0V0E9PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
    client-key-data: LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktLS0tLQpNSUlFcEFJQkFBS0NBUUVBbXlhTTlqa1hiMGtTNG5GR3pnS2pDTjJxcUpIM3YzV1lNNFdic3IvcGJVRE5EU2FFCmNocWtVL3VtdWliMnFUMzNTVWlxcEp6YWJBVE9TYVhqS09rMVR4YWdmMktJUUlOOUx4aFdXcStyzdVTWlSNQp4ejJqS0pWUEpXTUxYb0poTThsN2dJc2NpQ2NYU09FM3hOTmdVQlhIbWRjR096bFpiejZsQ3c9PQotLS0tLUVORCBSU0EgUFJJVkFURSBLRVktLS0tLQo=
```
