On master node

Print the command to run on the worker nodes.
`kubeadm token create --print-join-command`

This will print something on the command line. Copy and paste on the worker nodes.

After this, go back to the master node and run `kubectl get node` and the workers should now be listed.

