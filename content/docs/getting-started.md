+++
title = 'Getting Started'
date = 2024-09-04T14:15:24+02:00
draft = false
type = 'docs'
prev = '/'
next = 'components/'
+++

To play around I suggest using [multipass](https://multipass.run/) to create a few Ubuntu VMs.
You can use [./hack/clusterplz](./hack/clusterplz) to create a cluster of 2 nodes easily using multipass.
Skate only supports private key authentication for now, so make sure your nodes are set up to allow your key.

```shell {filename=Shell}
# assumes an ssh priv pub key combo ~/.ssh/id_rsa & ~/.ssh/id_rsa.pub that it will add to the vm host's authorized_keys
# create a hack/.clusterplz.env file with SSH_PRIVATE_KEY and SSH_PUBLIC_KEY set to override this
./hack/clusterplz create
```

BTW: you can use `./hack/clusterplz restore` to restore a clean snapshot of the nodes if things get messed up.

Install the skate CLI:

```shell
# Get list of latest release binaries
curl -s https://api.github.com/repos/skateco/skate/releases/latest | grep "browser_download_url.*tar.gz" | cut -d : -f 2,3 | tr -d \\\" | tr -d "[:blank:]"|grep -v skatelet
```

Download the `skate` binary for your platform and architecture.

Put it in your path.

Now, let's register a cluster:

*Note: Change ~/.ssh/id_rsa to the path to the private key that can access your nodes*.
*Should be the same that was used with the `clusterplz` script*

```shell
skate create cluster my-cluster --default-user $USER --default-key ~/.ssh/id_rsa
```

Add the nodes:

```shell
# get our node ips
> ./hack/clusterplz ips
192.168.76.11
192.168.76.12

# Provision each node with skatelet and everything else that skate needs
# NOTE: The --subnet-cidr has to be unique per node
> skate create node --name node-1 --host 192.168.76.11 --subnet-cidr 20.1.0.0/16
...
... much install

> skate create node --name node-2 --host 192.168.76.12 --subnet-cidr 20.2.0.0/16

...
... much install

```

Ok, now we should have a 2 node cluster that we can deploy to.

```shell
# list the nodes to be sure
> skate get nodes
NAME                            PODS        STATUS    
node-1                          2           Healthy   
node-2                          2           Healthy  
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
# 192.168.76.11 is a node-ip from hack/clusterplz ips
curl --header "Host: nginx.example.com" --insecure  http://192.168.76.11
```

```shell
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```
Great Success!!

Now you've deployed a webservice available via the ingress.
