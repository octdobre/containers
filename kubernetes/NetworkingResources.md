# Kubernetes Networking Resources



## Services

It exposes a set of pods clients on the network.


There are 3 types of services:
* NodePort -> todo
* ClusterIP -> todo
* LoadBalancer -> todo

```
kubectl apply -f enviroconfigs-service.yaml
```
```
kubectl delete svc/deployment
```

:point_right: :link: [Services](https://kubernetes.io/docs/concepts/services-networking/service/)