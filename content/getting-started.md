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

```shell
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

*Note: Change ~/.ssh/id_rsa to the path to the private key that can access your nodes*

```shell
skate create cluster my-cluster --default-user $USER --default-key ~/.ssh/id_rsa
```

Add the nodes:

```shell
> ./hack/clusterplz ips
192.168.76.11
192.168.76.12

# The --subnet-cidr has to be unique per node
> skate create node --name node-1 --host 192.168.76.11 --subnet-cidr 20.1.0.0/16
...
... much install

> skate create node --name node-2 --host 192.168.76.12 --subnet-cidr 20.2.0.0/16

...
... much install

```

Ok, now we should have a 2 node cluster that we can deploy to.

```shell
> skate get nodes
NAME                            PODS        STATUS    
node-1                          2           Healthy   
node-2                          2           Healthy  
```

Create a deployment

```shell
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
skate get deployment -n my-app
```

