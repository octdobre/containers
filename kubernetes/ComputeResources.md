# Kubernetes Compute Resources

Kubernetes compute resources represent resources that perform computation tasks.

A few examples of compute resources are:
* Pods
* Jobs

## Pods

A pod is a basic execution unit in a kubernetes. 
A pod can host one or multiple containers. It is a layer abover a container.

The difference between a pod and a Docker container is that
all containers created under a pod share the same network ports and IP.

Example yaml for creating a singular pod:
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```
## Controllers

A controller is a resource that manages other resources.

A controller is a form of `control-loop` meaning that it always runs and makes sure that resource that it manages
is operational. Pods are usually created under a controller resource that manages the pod.

Some of the things that a controller manages:
* On what Kubernetes Node the pod should be deployed
* Making sure that the pod is healty(enaugh cpu and ram)
* It manages replication of pods
* It manages rollout of pods


### Deployment

A deployment is a high level resources that mainly manages pods but also other resources.

A deployment manages a resource called `ReplicaSet` which itself is directly responsible for 
creating the pods. 

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

### ReplicaSet

A replicaset is directly responsible for making sure that a certain number of replicas of a pod are running
at a specific time. If a pod dies then a new one is going to be created to ensure the number of replicas.

It is not usual to create replicaset by itself but only through a deployment.

### StatefullSet

### DaemonSet



## Documentation :books:

:point_right: :link:[Pods](https://kubernetes.io/docs/concepts/workloads/pods/)

:point_right: :link:[Controllers](https://kubernetes.io/docs/concepts/architecture/controller/)

:point_right: :link:[Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)

:point_right: :link:[ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)

