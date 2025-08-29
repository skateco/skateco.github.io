+++
title = 'Clusters'
date = 2025-08-29T12:58:20+02:00
draft = false
type="docs"
+++

A skate cluster is composed of one or more servers,
or nodes as they'll be called here.
Cluster configuration is stored in $HOME/.skate/config.yaml.

Creating a cluster is simple:

skate create cluster my-cluster

Most other skate commands need a cluster to be in context:

skate config use-cluster my-cluster

To later change which cluster is active:

skate config use-cluster another-cluster

Once you have a cluster in context, you can then add nodes. 
See [](docs/components/nodes) for more information.