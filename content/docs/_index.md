+++
title = 'Documentation'
date = 2024-09-13T13:16:36+02:00
draft = false
+++

👋 Hej! Welcome to the Skate documentation!

<!--more-->

## What is Skate?

Skate is a minimalist container platform designed with simplicity in mind.

Deploy to skate using the same k8s manifests you already know, but skip the enormous complexity of running k8s yourself.

## Features


- **Kubernetes Manifests** - Deploy using the same syntax you use in your day job, without the burden of running k8s yourself.
- **Low resource** - Skate is written in rust, runs no daemon of it's own and uses minimal resources.
- **Battery-included** - Ingress resources get LetsEncrypt TLS by default.


## Install

```shell
curl -sL https://raw.githubusercontent.com/skateco/skate/refs/heads/main/hack/install-skate.sh |bash
```


## Questions or Feedback?

{{< callout emoji="❓" >}}
Skate is still in active development.
Have a question or feedback? Feel free to [open an issue](https://github.com/skateco/skate/issues)!
{{< /callout >}}

## Next

Dive right into the following section to get started:

{{< cards >}}
{{< card link="getting-started" title="Getting Started" icon="document-text" subtitle="Learn how to create a cluster using Skate" >}}
{{< /cards >}}

