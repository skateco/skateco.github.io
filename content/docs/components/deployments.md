+++
title = 'Deployments'
date = 2024-09-13T12:58:20+02:00
draft = false
type="docs"
+++


```shell
cat <<EOF | skate apply -f -
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  namespace: foo
spec:
  replicas: 3
  selector: {}
  template:
    spec:
      containers:
      - name: echo
        image: nginx:1.14.2
```

## DNS

All pods in a deployment automatically receive a dns listing under `<name>.<namespace>.pod.cluster.skate`.
See [Services](/docs/components/services) for more information on exposing the deployment as a service which is more reliable.

## Healthchecks

`podman kube play` supports `livenessProbe` in the pod manifest.
The best way to ensure that http traffic stops being routed to an unhealthy pod is to combine that with `restartPolicy`
of `Always` or `OnFailure`.

**Traffic will only start being routed to your pod once all containers in the pod are healthy.**

NOTE: using the `httpGet` probe results in podman trying to run `curl` within the container.
With `tcpSocket` it looks for `nc`.


