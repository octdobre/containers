# Authentication

In order to connect to the Kubernetes API and manage resources, a connection must be established using the command line.

The configuration that the command line tool uses is situated in a configuration file.

Check more information for both Windows and Linux as to where this configuration file exists.

The configuration file is in JSON format and can be modified with a text editor.

The configuration can also be updated using the kubernetes command line tool using the following commands.

Create a cluster:
```
kubectl config set-cluster <name_of_cluster> --server=<url to server>
```

Set credentials:
```
kubectl config set-credentials <name_of_the_cluster> --token=<token value>
```

Kubernetes credentials are stored under a specifi context.
The configuration file can hold multiple context.
Switching between contexts, switches to which cluster the commands are being sent to.

Create the context:
```
kubectl config set-context <name_of_context> --cluster <name_of_the_cluster> --namespace <namespace_name> --user <name_of_kubernetes_user>
```

To switch to a differnt context.
```
kubectl config use-context newcontext
```

To switch to a different namespace in the context.
The namespace will become the default WORKING namespace.
```
kubectl config set-context --current --namespace=<insert namespace here>
```