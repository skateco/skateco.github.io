+++
title = 'Daemonsets'
date = 2024-09-13T12:58:20+02:00
draft = false
type="docs"
+++


```shell
cat <<EOF | skate apply -f -
---
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx-daemonset
  namespace: foo
  labels:
    app: nginx
spec:
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
EOF
```

See [Deployments](/docs/components/deployments) for more information regarding dns and healthchecks.

