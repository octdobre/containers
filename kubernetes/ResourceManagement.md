# Kubernetes Resources

Kubertnetes is a service which manages a collection of resources.

A resource is either a pod, deployment, service, network bridge or any other type.

Resources are created using .yaml files how it should be created.

Interraction with Kubernetes is done using an API.

Yaml files are uploaded using the API and Kubernetes then takes care of creating the resource.

Kubernetes does not guarantee that a resource will be created but it will try its best to ensure it. Otherwise it will display an appropriate error.

Kubernetes resources are not created immediatelly like Docker but the creation of resources is eventually consistent meaning that they will be created at a later time.

The main types of resources that Kubernetes manages:
* Compute
* Storage
* Network

## Creating resources

Resources are always created from yaml files.

Define the resource in the yaml file and run the CMD command to `apply` the yaml to the cluster.

Kubernetes will take care to create all of the resources in the yaml if possible.

```
kubectl apply -f myresource.yaml
```


## Nodes

A kubernetes node is an instance of a machine on which resources can be deployed.

Get the nodes:
```
kubectl get nodes
```
```
kubectl label node docker-desktop storage=SSD
```
```
kubectl describe node 
```

```
nodeSelector:
  storage: SSD
```

## Namespaces

A namespace is a resource to group and isolate other resources.

Kubernetes also has a 'default' namespaces where resources are placed that do not specify a namespace.

Example of a yaml to create a namespace:
```
apiVersion: v1
kind: Namespace
metadata:
  name: test
  labels:
    name: test
```

Commands to get all resources from the default namespace:
```
kubectl get all
```
Commands to get all resources from a specific namespace:
```
kubectl get all -<name_of_the_namespace>
```

When a namespace is deleted, all child resources are deleted also.

To work with Namespace, you need to add --namespace flag to k8s commands.

```
kubectl create -f deployment.yaml --namespace=custom-namespace
```

To create a namespace:
```
kubectl create namespace <name>
```

To delete a namespace:
```
 kubectl delete namespace/deployment
```

View all namespaces
```
kubectl get namespaces
```

Switch to a different namespace as WORKING namespace:
```
kubens <mynamespace> -> switch to namespace
```

## Configuration(Clusters, Namespaces)

View current context and namespace
```
kubectl config get-contexts
```

Change namespace
```
kubectl config set-context --current --namespace=<name>
```

Change context
```
kubectl config use-context <name>
```

## Documentation 

:point_right: :link: [Namespaces](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)
