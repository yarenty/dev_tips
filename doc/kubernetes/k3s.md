---
title: K3S
main_link: https://github.com/openfaas/faasd
keywords: [k3s, kubernetes, install, raspberry, openfaas, cli]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# K3S

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

`#k3s` `#kubernetes` `#install` `#raspberry` `#openfaas` `#cli`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes
<!-- auto-split by article_split.py -->
> Auto-split: 5 additional top-level heading(s) extracted into sibling files:
> - [Omit sudo if you wish, then move the arkade binary to /usr/local/bin/](omit_sudo_if_you_wish_then_move_the_arkade_binary_to_usr_loc.md)
> - [Once Proxying you can navigate to the below](once_proxying_you_can_navigate_to_the_below.md)
> - [Build a local Docker image and push it to the Docker Hub](build_a_local_docker_image_and_push_it_to_the_docker_hub.md)
> - [Deploy the function using the image](deploy_the_function_using_the_image.md)
> - [Now invoke your function](now_invoke_your_function.md)


<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# K3S


https://k3s.io/


Lightweight Kubernetes
The certified Kubernetes distribution built for IoT & Edge computing

This won't take long…


```bash
curl -sfL https://get.k3s.io | sh -
# Check for Ready node,
takes maybe 30 seconds
k3s kubectl get node
```



## Why Use K3s
Perfect for Edge
K3s is a highly available, certified Kubernetes distribution designed for production workloads in unattended, resource-constrained, remote locations or inside IoT appliances.

Simplified & Secure
K3s is packaged as a single <40MB binary that reduces the dependencies and steps needed to install, run and auto-update a production Kubernetes cluster.

Optimized for ARM
Both ARM64 and ARMv7 are supported with binaries and multiarch images available for both. K3s works great from something as small as a Raspberry Pi to an AWS a1.4xlarge 32GiB server.


image::{docdir}/../img/kubernetes/how-it-works-k3s.svg[]



## Intro

```bash
sudo k3s server &
# Kubeconfig is written to /etc/rancher/k3s/k3s.yaml
sudo k3s kubectl get node

# On a different node run the below. NODE_TOKEN comes from /var/lib/rancher/k3s/server/node-token
# on your server
sudo k3s agent --server https://myserver:6443 --token ${NODE_TOKEN}
```








## Rapsberry

https://alexellisuk.medium.com/walk-through-install-kubernetes-to-your-raspberry-pi-in-15-minutes-84a8492dc95a


Here’s something you can do before work, with your morning coffee, or whilst waiting for dinner to cook of an evening. And there’s never been a better time to install Kubernetes to a Raspberry Pi, with the price-drop on the 2GB model — perfect for containers.

You can buy a single RPi and still have a lot of fun, here I bought 4x 2GB nodes
I’ll show you how to install Kubernetes to your Raspberry Pi in 15 minutes including monitoring and how to deploy containers.
Updates:
Dec 2020 — added cmdline.txt instructions for cgroups and ssh-copy-id
Jan 2021 — added multi-arch faas-cli publish command instead of faas-cli up to use new templates and Docker buildx
Mar 2021 — Raspbian is now Raspberry Pi OS
The bill of materials
I’ll keep this quite simple.
Raspberry Pi 4, with 2GB or 4GB RAM — the 2GB is the best value, 4GB is best if you don’t plan on doing clustering.
SD card — 32GB recommended, larger is up to you, but Kubernetes writes to disk a lot and could kill a card, so I tend to prefer buying more smaller cards.
Power supply — you need the official supply, I know it’s expensive, but that’s for a reason. Don’t be cheap because you’ll buy twice.
Docker Desktop — if you want to build your own images, you need to cross-compile them from a PC with buildx, do not install docker on your nodes.
If you’d like some links, you can find them in my home-lab post: Kubernetes Homelab with Raspberry Pi and k3sup.
Flash the initial OS
There are so many ways to install an Operating System, but I recommend Raspberry Pi OS and the Lite edition which ships without a UI.
Once you download the image, you can use Etcher.io from our friends at Balena to flash it without even unzipping it. How cool is that?
Before you boot up that RPi, make sure you create a file named ssh in the boot partition. If on a Mac you'll see that gets mounted for you as soon as you eject and re-insert the SD card.
Connect for the first boot
Now connect to the Raspberry Pi over your local network, it will show up as raspberry.local, but if you can’t connect for some reason, then install nmap and run nmap -sP 192.168.0.0/24 to run a network scan.
Change the password with passwd pi.
Run raspi-config and change the memory split to 16mb, so that we have all the RAM for Kubernetes, believe me, it needs it.
There’s one more change that’s essential for k3s. Add the following to /boot/cmdline.txt, but make sure that you don’t add new lines.
cgroup_enable=cpuset cgroup_memory=1 cgroup_enable=memory
Copy or create an SSH key
k3sup uses password-less login by default, so that means you can run it from a script or automation without human intervention.
Copy your SSH key to the Raspberry Pi with:
ssh-copy-id pi@raspberrypi.local
If you have no SSH key on your local computer yet, then run ssh-keygen
Get your CLI tools
You do not need to log into your Raspberry Pi again. All tools will be installed on your client (i.e. your laptop) and the RPi will be accessed remotely as a server.
arkade — a hassle-free way to get Kubernetes apps and CLIs
kubectl — the Kubernetes CLI
k3sup — the Kubernetes (k3s) installer that uses SSH to bootstrap Kubernetes
arkade is a portable Kubernetes marketplace which makes it easy to install around 40 apps to your cluster, without worrying about all the gory details and configuration options. arkade also “does the right thing” for instance:
An app like OpenFaaS uses a helm chart
A tool like the Kubernetes dashboard only uses plain YAML manifests
Linkerd for example prefers to use a CLI
arkade abstracts that all away from the user with around 40 apps on offer. On top of that, if an app like Istio is known not to work on your device, it will block you from doing the wrong thing.
We can also use it to download k3sup and kubectl:
