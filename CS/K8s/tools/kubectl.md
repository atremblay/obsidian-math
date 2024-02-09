Command line Tool to talk to the cluster

People call this "cube ctl", "cube control" or "cube cuttle".

This is the most powerful tool to interact with the [[../Architecture/Master#1. API server|API server]] on the [[../Architecture/Master|master node]]

You can manage components solely via the command line, but that's not optimal. Instead, the user of configuration files is much much better.

## Common commands

| command                                        | description                      |
| ---------------------------------------------- | -------------------------------- |
| `kubectl create <object type> <instance name>` | Create a component (imperative)  |
| `kubectl create -f config.yaml`                | Create a component (declarative) |
| `kubectl delete <object type>/<instance name>` | Delete a component (imperative)  |
| `kubectl get ...`                              | Basic Information                |
| `kubectl describe ...`                         | aggregated detailed infos        |
| `kubectl logs ...`                             | logs of container                |


