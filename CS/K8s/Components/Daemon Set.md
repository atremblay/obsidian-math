Suppose that you play with the number of [[Node|nodes]]. You would have to go into the [[Deployment]] config and change the number of replicas. This is not practical. Furthermore, it's unclear how to distribute all the [[Pod|pods]] among all the newly created [[Node|nodes]]. DaemonSet are made just for that.

- Calculates how many Replicas are needed based on existing [[Node|nodes]]
- Deploys just 1 replica per Node
- When [[Node|nodes]] are added, [[Pod|pods]] are added to them
- When [[Node|nodes]] are removed, those [[Pod|pods]] are garbage collected
- Meaning that we do not need to defined the replica count
- Automatically scales up & down
- Garanties to have only 1 pod replica per node