# Master node status `NotReady`

The [[networking/Container Networking Interface#Solutions|CNI plugin]] has not been installed.


# \[preflight\] Some fatal errors occurred

```
azureuser@worker-node-1:~$ sudo kubeadm join 10.1.0.4:6443 --token bclzqa.939krqkcga8rfw99 --discovery-token-ca-cert-hash sha256:7bedaf398d08daf8dca4567de3a1cfe06c3ab8f886b949641b7761b57c21a768 
[preflight] Running pre-flight checks
error execution phase preflight: [preflight] Some fatal errors occurred:
	[ERROR FileAvailable--etc-kubernetes-kubelet.conf]: /etc/kubernetes/kubelet.conf already exists
	[ERROR Port-10250]: Port 10250 is in use
	[ERROR FileAvailable--etc-kubernetes-pki-ca.crt]: /etc/kubernetes/pki/ca.crt already exists
```

The error you're encountering, `[ERROR FileAvailable--etc-kubernetes-kubelet.conf]: /etc/kubernetes/kubelet.conf already exists`, suggests that the Kubernetes setup process is being obstructed because the `kubelet.conf` file already exists on your worker node. This situation usually occurs if you've previously attempted to join the node to a cluster, or if there's some leftover configuration from an earlier setup.

To resolve this issue, you can follow these steps:

1. **Reset the Kubernetes Node**: Before you join the node to the cluster again, you should reset the Kubernetes installation on the node. This can be done using the command:
   ```bash
   sudo kubeadm reset
   ```
   This command will clean up the node's configuration, removing files and settings related to the Kubernetes cluster.

2. **Confirm Removal of Kubernetes Configuration**: After running `kubeadm reset`, it's a good practice to manually check if the `/etc/kubernetes/` directory has been cleaned up, especially the `kubelet.conf` file. If it still exists, you can manually remove it:
   ```bash
   sudo rm -rf /etc/kubernetes/
   ```

3. **Rejoin the Node to the Cluster**: Now that the node has been reset and the conflicting configuration file has been removed, you can attempt to join the node to the cluster again with your original `kubeadm join` command.

4. **Troubleshooting**: If you continue to encounter issues, it might be helpful to inspect logs or run the join command with increased verbosity for additional details. For example:
   ```bash
   sudo kubeadm join [your-join-parameters] --v=5
   ```

Remember, when working with Kubernetes, it's essential to ensure that your environment is clean before setting up or joining nodes to a cluster, especially if you're reusing machines that might have old configurations.

