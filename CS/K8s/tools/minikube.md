
The [[../Architecture/Master|master node]] and [[../Architecture/Worker|Worker]] are running on the same machine. Use `minikube`. It will create a virtual box and start the master/slave processes. 
Need to install the actual virtualization software. 

`minikube start --vm-driver=hyperkit`

It only starts/stop the cluster. Managing the config is done via [[#kubectl]]

