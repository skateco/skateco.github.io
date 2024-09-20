+++
title = 'Secrets'
date = 2024-09-13T12:58:25+02:00
draft = false
type="docs"
+++

Secrets are scheduled across all nodes in the cluster, stored as [podman secrets](https://docs.podman.io/en/latest/markdown/podman-secret-create.1.html)

```shell
cat <<EOF | skate apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: test
  namespace: my-app
type: Opaque
data:
  username: dXNlcg==
  password: NTRmNDFkMTJlOGZh
EOF
```

`podman kube play` actually stores the full yaml as the secret value, and supports injecting or mounting secrets into containers.

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

