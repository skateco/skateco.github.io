+++
title = 'Getting Started'
date = 2024-09-04T14:15:24+02:00
draft = false
type = 'docs'
prev = '/'
next = 'components/'
+++

Install the `skate` binary for your platform and architecture.

```shell
curl -sL https://raw.githubusercontent.com/skateco/skate/refs/heads/main/hack/install-skate.sh | bash
```

To play around I suggest using Skate in Docker (sind) to run a local cluster.

```shell {filename=Shell}
curl -sL https://raw.githubusercontent.com/skateco/skate/refs/heads/main/hack/install-sind.sh | bash
```

Create the skate-in-docker cluster:

```shell
sind create --ssh-private-key ~/.ssh/<some-private-ssh-key> --ssh-public-key ~/.ssh/<some-public-ssh-key>
```


> **_BTW:_**
> You can run `sind remove` followed by `sind create ...` again to run a new cluster if things get messed up.



> **_NOTE:_**
> `sind` automatically creates the cluster via skate.
> To do this normally you have to run the following:
> 
> ```shell
> skate create cluster my-cluster --default-user <valid ssh user> --default-key ~/.ssh/<some ssh public key>
> skate config use-context my-cluster
> ```
>
> **_NOTE:_**
> If you don't specify `--default-key` (or `--key` when doing `skate create node`) skate will check the ssh agent for identities
> 
> Now add the nodes:
> 
> ```shell
> 
> skate create node --name node-1 --subnet-cidr 20.1.0.0/16 --host <ip> --peer-host <ip wrt other hosts>
> skate create node --name node-2 --subnet-cidr 20.2.0.0/16 --host <ip> --peer-host <ip wrt other hosts>
> ```



Ok, now we should have a 2 node cluster that we can deploy to.

```shell
# list the nodes to be sure
> skate get nodes
NAME                            PODS        STATUS    
sind-node-1                          2           Healthy   
sind-node-2                          2           Healthy  
```

Create a deployment

```shell
# creates an nginx deployment with 2 replicas
cat <<EOF | skate apply -f -
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  namespace: my-app
spec:
  replicas: 2
  template:
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
EOF
```

Check the deployment

```shell   
skate get deployment

NAMESPACE  NAME   READY  UPTODATE  AVAILABLE  AGE 
my-app     nginx  2/2    2         2          45s 
```

Now you can create a service:

```shell
cat <<EOF | skate apply -f -
---
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

```shell   
skate get service

NAMESPACE  NAME   CLUSTERIP  EXTERNALIP  PORTS  AGE 
my-app     nginx  -          -           80     10s 
```

And finally an ingress:

```shell
cat <<EOF | skate apply -f -
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: public
  namespace: my-app
  annotations:
    # since the vm cluster isn't public, letsencrypt wont work, so turn off redirect so we can test
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
spec:
  rules:
  - host: nginx.example.com
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

Now let's do a quick request against the cluster:
```shell
# 192.168.76.11 is a node-ip from `sind ports`
curl --header "Host: nginx.example.com" --insecure  http://192.168.76.11
```

You should see something like this:
```shell
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
...
```
Great Success!!

Now you've deployed a webservice across 2 nodes available via the ingress.
