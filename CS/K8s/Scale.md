Scaling an application can be done via the `replicas` count in a config file for a permanent change or with this for a temporary change

```
kubectl scale <component type> <component name> —-replicas=4

E.g.
kubectl scale deployment nginx-deployment —-replicas=4
```
