---
aliases:
  - CNI
url:
  - https://kubernetes.io/docs/concepts/cluster-administration/addons/
---

> [!Summary] 
> Container Network Interface is a set of rules to are design for Pod-to-Pod communcation


K8s imposes requirements for a Container Network Interface to be usable
1. Every [[../Components/Pod|Pod]] gets it own unique IP address **across all nodes**. It must be unique in the whole cluster.
2. Pods on the same Node can communicate with that IP address
3. Pods on a different [[../Components/Node|Node]] can communication with that IP address without NAT (Network Address Translation)


![[../images/Pasted image 20240103162636.png|400]]


- The [[../../networking/VPC|virtual private cloud]] provides a range of IP adresses that can be used by the nodes (e.g. 172.31.0.0/16).
- In each [[../Components/Node|Node]], a private network exists. Pods will use an IP address in that private network. The IP addresses for the Pods are defined by the [[#Solutions]] that implements CNI. (e.g. 10.32.0.0/12)

```mermaid
flowchart LR
    subgraph node1["Node (172.31.1.10)"]
        pod1["Pod (10.32.1.10)"]
        pod2["Pod (10.32.1.11)"]
        bridge1["Bridge 10.32.1.0/24"]
        pod1 --- bridge1
        pod2 --- bridge1
    end
    subgraph node2["Node (172.31.1.11)"]
        pod3["Pod (10.32.2.10)"]
        bridge2["Bridge 10.32.2.0/24"]
        pod3 --- bridge2
    end
    node1 --- lan["LAN (172.31.0.0)"]
    node2 --- lan
    
```



## Solutions
https://kubernetes.io/docs/concepts/cluster-administration/addons/

Third party solutions are:

- Flannel
- WeaveWorks
- Cilium
- VMware NSX