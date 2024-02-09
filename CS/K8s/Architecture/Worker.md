---
aliases:
  - worker node
  - worker nodes
---
# Processes

- These are the nodes/servers that execute the work


3 processes must be installed on every Node (via Ansible?). The worker nodes are doing the heavy lifting, thus need good compute resources.

## 1. Container runtime (e.g. Docker, ContainerD, cri-o)

Since the [[../Components/Pod|pods]] are running the container, the node needs to have the container runtime.

## 2. Kubelet

- interacts with both the container and [[Node|node]]
- Starts pod with a container inside. Command comes from the [[Master|master node]]

## 3. Kube Proxy

Intelligent forwarding logic inside. If an application can be answered by another application (pod) on the same node, it will prioritize that one instead of having the overhead of going to another node
 