---
title: Now invoke your function
main_link: https://github.com/openfaas/faasd
keywords: [now-invoke-your-function, kubernetes, k3s, raspberry, learn, openfaas]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/kubernetes/k3s.md` by `article_split.py`. Heading: **Now invoke your function**.

# Now invoke your function

**Main link:** <https://github.com/openfaas/faasd>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#now-invoke-your-function` `#kubernetes` `#k3s` `#raspberry` `#learn` `#openfaas`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Now invoke your function
faas-cli invoke my-api
Doing multi-arch right
If you’re running 64-bit Ubuntu on your Raspberry Pi, then you’ll need to use --platforms linux/arm64 instead. You can also build for multiple platforms by adding them with a comma between each. Just run faas-cli publish --help to find out an example of how.
You can also edit the function’s code and then run faas-cli publish then faas-cli deploy again:
Contents of: my-api/handler.go
package function
import (
"net/http"
"github.com/openfaas-incubator/go-function-sdk"
)
func Handle(req handler.Request) (handler.Response, error) {
return handler.Response{
Body:       []byte(`Run k3s on your RPi!`),
StatusCode: http.StatusOK,
}, nil
}
Find out more about OpenFaaS at openfaas.com
You can also see your functions on the Kubernetes Dashboard:

Get a public IP for your cluster
You can get a public IP for your cluster via a tunnel the inlets-operator for Kubernetes.

The inlets-operator gives you LoadBalancers with public IPs just like on a public cloud provider
Expose your local OpenFaaS functions to the Internet
Expose Your IngressController and get TLS from LetsEncrypt and cert-manager
Build your own homelab cluster for self-hosting
If you’d like to build a resilient homelab, that has multiple master nodes (servers) and uses faster, more reliable network storage, checkout my new workshop available on Gumroad.
In the workshop, you’ll learn how to configure a netbooting server, then boot your Raspberry Pi directly from the network, from there you can install K3s and explore different applications you can add on top. High Availability is essential for self-hosting, so that your cluster can tolerate a host failure.

Wrapping up and next steps
If you want to take things further, you can start adding additional nodes into the cluster, to extend its capacity and to give redundancy.
Upgrade your Raspberry Pi 4 with a NVMe boot drive
Five years of Raspberry Pi Clusters
Star or fork k3sup and arkade on GitHub ⭐️
You can connect with the OpenFaaS community — to talk about Kubernetes, ARM, Raspberry Pi clusters and serverless. Join our Slack workspace today.





## Introduction to Kubernetes on Edge with K3s

https://learning.edx.org/course/course-v1:LinuxFoundationX+LFS156x+2T2021/home




## Pi

The following are good resources to use:

Raspberry PI Foundation: products and hardware

https://www.raspberrypi.org/products/

BitScope: Cluster Blade product for industrial applications

https://www.bitscope.com/blog/JK/?p=JK38B


Pimoroni: sensors and add-ons for Raspberry Pi.

https://pimoroni.com/


The following are complementary resources from Alex Ellis:

Watch a video from KubeCon: The Past, Present, and Future of Kubernetes on Raspberry Pi
https://www.youtube.com/watch?v=jfUpF40--60


Follow a 15-minute guide for K3s on Raspberry Pi with applications: Walk-through — install Kubernetes to your Raspberry Pi in 15 minutes

https://alexellisuk.medium.com/walk-through-install-kubernetes-to-your-raspberry-pi-in-15-minutes-84a8492dc95a


## K3s in production

The following are some of the resources that you can use for self-study:

Learn more about K3s in the project documentation
https://k3s.io/

Learn about k3sup as used in the course - k3sup on GitHub - install K3s over SSH
https://k3sup.dev/


Learn about ArgoCD
https://argoproj.github.io/argo-cd/


Learn about Flux
https://fluxcd.io/

The following are complementary resources from Alex Ellis:

Learn to set up K3s on clouds without load-balancers: Bare-metal Kubernetes with K3s
https://blog.alexellis.io/bare-metal-kubernetes-with-k3s/

Set up K3s using a database and load-balancer: Set up Your K3s Cluster for High Availability on DigitalOcean
https://rancher.com/blog/2020/k3s-high-availability

## Functions as Service


Learn everything you need to know about Serverless on Kubernetes with the Edx course: Introduction to Serverless on Kubernetes (LFS157x).
https://www.edx.org/course/introduction-to-serverless-on-kubernetes


faasd offers a way to run OpenFaaS without also having to manage Kubernetes, and may provide a suitable alternative, especially with resource constrained devices.
https://github.com/openfaas/faasd
https://www.openfaas.com/


## Kubernetes

There are numerous resources to learn more about Kubernetes:

Browse the CNCF Landscape
https://landscape.cncf.io/

Check out the Kubernetes documentation and tutorials
https://kubernetes.io/docs/home/
https://kubernetes.io/docs/tutorials/

Take a Kubernetes course offered by The Linux Foundation
Gain a Kubernetes certification: Certified Kubernetes Administrator (CKA), Certified Kubernetes Application Developer (CKAD), Certified Kubernetes Security Specialist (CKS)
Learn more about K3s.
