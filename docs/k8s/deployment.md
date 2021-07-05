# deployment

!!! note "Work in progress"
    This document is a work in progress.

### Liveness probes 
`Liveness probes` are used in Kubernetes to know when a pod is alive or dead. A pod can be in a dead state for a variety of reasons; Kubernetes will kill and recreate the pod when a liveness probe does not pass.

### Readiness probes 
`Readiness probes` are used in Kubernetes to know when a pod is ready to serve traffic. Only when the readiness probe passes will a pod receive traffic from the service; if a readiness probe fails traffic will not be sent to the pod.
