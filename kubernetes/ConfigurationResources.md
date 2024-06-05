# Kubernetes Configuration Resources


## ConfigMaps

It is a resources used to store text and byte data.

It can be consumed by pods as environment variables, command line arguments, 
or they can be mounted as files to the file system of the pod.

Configmaps that contain files are mounted as simlinks and no files directly and the real path is different.

If a configmap is updated while pods are running, Kubernetes eventually updates the simlink to the new files.

## Secrets

Same confept as configmaps.

Reside only inside a namespace.

Must exist before a pod is created.

Can only be mounted to pods in the same namespace.

The size limit of a secret is 1Mb.




## Documentation

:point_right: :link:[ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/)

:point_right: :link: [Secret](https://kubernetes.io/docs/concepts/configuration/secret/)