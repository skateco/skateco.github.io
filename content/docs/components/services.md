+++
title = 'Services'
date = 2024-09-13T12:58:12+02:00
draft = false
type="docs"
+++


Skate services are [Linux Virtual Server](https://en.wikipedia.org/wiki/Linux_Virtual_Server) backed, enabling cross node
load balancing and service discovery.

The following will create a service listening on port 80 with name `nginx.my-app.svc.cluster.skate` pointing towards all pods in the deployment
`nginx` in namespace `my-app` on port 80.

```shell
cat <<<EOF | skate apply -f -
apiVersion: v1
kind: Service
metadata:
  name: nginx
  namespace: my-app
spec:
  selector:
    app.kubernetes.io/name: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
EOF
```
