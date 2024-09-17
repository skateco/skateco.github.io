+++
title = 'Deployments'
date = 2024-09-13T12:58:20+02:00
draft = false
type="docs"
+++


```shell
cat <<<EOF | skate apply -f -
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

## Injecting secrets

Secrets within the same namespace can be injected via:
```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  namespace: my-app
spec:
  ...
  template:
    spec:
      containers:
      - name: echo
        ...
        env:
        - name: TEST_SECRET
          valueFrom:
            secretKeyRef:
              name: test
              key: password

```
## Mounting Secrets

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  namespace: my-app
spec:
  ...
  template:
    spec:
      containers:
      - name: echo
        ...
        volumeMounts:
        - name: test-mount
          mountPath: /etc/foo
          readOnly: true
      volumes:
      - name: test-mount
        secret:
          secretName: test
          optional: false
```

