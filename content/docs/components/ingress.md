+++
title = 'Ingress'
date = 2024-09-13T12:58:09+02:00
draft = false
type="docs"
+++

Skate automatically runs an [ingress container](https://github.com/skateco/skate/tree/main/images/nginx-ingress) on every node.
TLS is automatically configured, default to lets-encrypts staging cluster.

Currently only the `Prefix` `pathType` is supported.
Supported annotations:
- `nginx.ingress.kubernetes.io/ssl-redirect` (default: `"true"`)
- `nginx.ingress.kubernetes.io/proxy-body-size` (default: `"5m"`)


```shell
cat <<EOF | skate apply -f -
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
  namespace: my-app
spec:
  rules:
  - host: foo.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: nginx.my-app
            port:
              number: 80
EOF
```

In order to enable production TLS, you need to instruct skate to use LetsEncrypt's production environment:
```shell
cat <<EOF | skate apply -f -    
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: default # can't be anything else
  namespace: skate # can't be anything else
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: your@email.com # not actually supported yet
EOF
```

Email is not actually supported yet, but it will be in the future.


