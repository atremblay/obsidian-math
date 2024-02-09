---
aliases:
  - config file
  - manifest
  - config files
  - manifests
---

Configuration files are yaml files that describe a component. They are also called Manifest.

You can store the config files in the same git repo as the code or in its own repo.

# Imperative vs Declarative

If we create a component only on the command line, we tell K8s what to do (`create`, `delete`, `update`). This is **imperative**.

Or we tell K8s what we want as an end result via a config file. This is **declarative**. K8s will figure out how to reach the desired state from the current state.

### Declarative
History of configurations
Infrastructure as Code in Git Repo
Collaboration and review processes possible
More transparent

### Imperative
Practical when testing
or for quick one-off tasks
or when just getting started

# Configuration file
The configuration file is a yaml file.


###### Pro tip: Quickly get a config template for a component

`kubectl create --help` will give a list of all the components that can be created on the fly. Some of them will have a `--dry-run` option that will print in the terminal a standard config/manifest file.
e.g.
`kubectl create deployment my-deployment-name --image=nginx -o yaml --dry-run=client`
will print (can be forwarded to a file)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: my-deployment-name
  name: my-deployment-name
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-deployment-name
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: my-deployment-name
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
status: {}
```

We can go a step further by providing parameters on the CLI

`kubectl create deployment my-deployment --image=nginx:1.20 --port=80 --replicas=3 --dry-run -o yaml > my-deployment.yaml`

###### Pro Tip: Quickly get a config template for a pod
[[Components/Pod|Pods]] cannot be created with `kubectl create`. Instead use `kubectl run my-pod --image=nginx=1:20 --labels="app=nginx,env=prod" --dry-run=client -o yaml > my-pod.yaml`


## Metadata
Example of meta data
```yaml
apiVersion: v1
kind: Deployment # Name of the component
```


## Specification

Each component (`kind` in [[#Metadata]]) has its own specification.
```yaml
spec:
    replicas: 2
    selector: ...
    template: ...
```

## Status
Automatically generated and added by K8s. K8s checks what is the desired state versus the actual state. This is the basis of the self-healing feature of K8s. 
The status is update continuously. We do not have to worry about that part.

## Selectors
- [ ] Aggregate information about Selectors and Labels

