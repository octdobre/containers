# Resource Management


A mechanism with wich Kubernetes can ensure that a pod is correctly started and is correctly running is the usade of a sidecar container.

A sidecar container as the name implies is a helper container which sits by the side of the container, usually in the same pod.

## Readiness probes

    One type of sidecar contaiener is the readiness container.
    It's purpose is to ensure that a pod is ready after it has started and it is ready to work.
    This type of container may start once and then be deleted if the main container has correctly started.


## Liveliness probes

    Another type of sidecar container is the liveliness probe.
    This container makes sure that the container is continuously running correctly.
    One example could be a web server that is running and a sidecar container that constantly pings the web server to assert a health check.
    