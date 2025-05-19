+++
title = 'Skate'
date = 2024-09-04T10:34:46+02:00
layout="hextra-home"
+++

{{< hextra/hero-badge >}}
  <div class="hx-w-2 hx-h-2 hx-rounded-full hx-bg-primary-400"></div>
  <span>Free, open source</span>
  {{< icon name="arrow-circle-right" attributes="height=14" >}}
{{< /hextra/hero-badge >}}

<div class="hx-mt-6 hx-mb-6">
{{< hextra/hero-headline >}}
K8s, but simple
{{< /hextra/hero-headline >}}
</div>

<div class="hx-mb-12">
{{< hextra/hero-subtitle >}}
A lightweight, multi-host, daemonless mini-pass that supports deploying kubernetes manifests.

{{< /hextra/hero-subtitle >}}
</div>

<div class="hx-mb-6">
{{< hextra/hero-button text="Get Started" link="docs/getting-started" >}}
</div>


{{< hextra/feature-grid >}}
{{< hextra/feature-card
title="Simple"
subtitle="Simple and easy to use, yet powerful and feature-rich."
class="hx-aspect-auto md:hx-aspect-[1.1/1] max-md:hx-min-h-[340px]"
image="images/hi5.png"
imageClass="hx-top-[40%] hx-left-[24px] hx-w-[180%] sm:hx-w-[110%] dark:hx-opacity-80"
style="background: radial-gradient(ellipse at 50% 80%,rgba(194,97,254,0.15),hsla(0,0%,100%,0));"
>}}
{{< hextra/feature-card
title="Manifests are all you need"
subtitle="Deploy with k8s manifests. Use what you already know."
class="hx-aspect-auto md:hx-aspect-[1.1/1] max-lg:hx-min-h-[340px]"
image="images/k8s.png"
imageClass="hx-top-[40%] hx-left-[36px] hx-w-[180%] sm:hx-w-[110%] dark:hx-opacity-80"
style="background: radial-gradient(ellipse at 50% 80%,rgba(142,53,74,0.15),hsla(0,0%,100%,0));"
>}}
{{< hextra/feature-card
title="Low Resource"
subtitle="No daemon, no control plane, leverages systemd ecosystem. Written in Rust."
class="hx-aspect-auto md:hx-aspect-[1.1/1] max-md:hx-min-h-[340px]"
image="images/cpu.png"
imageClass="hx-top-[40%] hx-left-[36px] hx-w-[100%] sm:hx-w-[100%] dark:hx-opacity-80"
style="background: radial-gradient(ellipse at 50% 80%,rgba(221,210,59,0.15),hsla(0,0%,100%,0));"
>}}
{{< /hextra/feature-grid >}}