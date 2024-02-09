---
aliases:
  - control plane
  - master node
---


# Processes
The same 3 processes as the [[Worker|worker nodes]] 

plus

4 processes on every master node (via Ansible?). The master node does not need a whole lot of resources. Each of these processes run in their own pods. 

## 1. API server
- cluster gateway
- Acts as a gatekeeper for authentication
- An external tool, such as [[../tools/kubectl|kubectl]], will interact with this.

## 2. Scheduler
Assign a [[../Components/Pod|pod]] to a [[../Components/Node|Node]] based on the required and available resources (info available in [[#4. ETCD|etcd]]). Command is sent to the slave’s [[Worker#2. Kubelet|kublet]] process

## 3. Controller Manager: 
- Detects cluster state change.
- e.g. Restart dead pods by telling the scheduler to send that request to the [[Worker#2. Kubelet|kubelet]]

## 4. ETCD 
ETCD is the cluster brain. Cluster changes get stored in the key/value store. 

The information here are used to answer these questions
- What resources are available?
- Did the cluster state change?
- Is the cluster healthy?


> [!important] 
> Application data is **NOT** stored in etdc

