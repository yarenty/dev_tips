---
title: Omit sudo if you wish, then move the arkade binary to /usr/local/bin/
main_link: https://get.arkade.dev
keywords: [omit-sudo-if-you-wish-then-move-the-arkade-binary-to-usr-loc, kubernetes, install, openfaas, arkade, ingress]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/kubernetes/k3s.md` by `article_split.py`. Heading: **Omit sudo if you wish, then move the arkade binary to /usr/local/bin/**.

# Omit sudo if you wish, then move the arkade binary to /usr/local/bin/

**Main link:** <https://get.arkade.dev>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#omit-sudo-if-you-wish-then-move-the-arkade-binary-to-usr-loc` `#kubernetes` `#install` `#openfaas` `#arkade` `#ingress`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Omit sudo if you wish, then move the arkade binary to /usr/local/bin/
curl -sSL https://get.arkade.dev | sudo sh
arkade get kubectl
arkade get k3sup
Did you know that you can also specify a version to arkade get? For example: arkade get kubectl --version 1.19.5
k3sup install can be used to install k3s as a server, to begin a new single-node cluster (that’s what we’ll do today). If you have multiple nodes, then the k3sup join command lets you add in additional agents or workers to expand the capacity.
Install Kubernetes with k3sup and k3s
k3s is a lightweight edition of Kubernetes made by Rancher Labs, it’s suitable for production, but also perfect for small devices like our Raspberry Pi. Its memory requirements are around 500MB for a server vs. around 2GB for kubeadm (upstream Kubernetes)
export IP="192.168.0.1" # find from ifconfig on RPi
k3sup install --ip $IP --user pi
In a few moments you’ll receive a kubeconfig file into your local directory, with an instruction on how to use it.
Find the node, and check if it’s ready yet
export KUBECONFIG=`pwd`/kubeconfig
kubectl get node -o wide
You can add -w to most kubectl commands to “watch” or “stream” the output status, so you can save on typing.
By default k3s comes with the metrics-server, which is used for Pod autoscaling and getting memory/CPU for pods and nodes:
kubectl top node
kubectl top pod --all-namespaces
Now let’s install one or two apps, run arkade install to see what's available, but not that not all projects in the CNCF landscape work on ARM devices.
arkade install --help
Available Commands:
argocd                  Install argocd
cert-manager            Install cert-manager
chart                   Install the specified helm chart
consul-connect          Install Consul Service Mesh
cron-connector          Install cron-connector for OpenFaaS
crossplane              Install Crossplane
docker-registry         Install a Docker registry
docker-registry-ingress Install registry ingress with TLS
gitea                   Install gitea
gitlab                  Install GitLab
grafana                 Install grafana
info                    Find info about a Kubernetes app
ingress-nginx           Install ingress-nginx
ingress-nginx           Install ingress-nginx
inlets-operator         Install inlets-operator
istio                   Install istio
jenkins                 Install jenkins
kafka-connector         Install kafka-connector for OpenFaaS
kong-ingress            Install kong-ingress for OpenFaaS
kube-image-prefetch     Install kube-image-prefetch
kube-state-metrics      Install kube-state-metrics
kubernetes-dashboard    Install kubernetes-dashboard
linkerd                 Install linkerd
loki                    Install Loki for monitoring and tracing
metrics-server          Install metrics-server
minio                   Install minio
mongodb                 Install mongodb
nats-connector          Install OpenFaaS connector for NATS
nfs-client-provisioner  Install nfs client provisioner
nginx-inc               Install nginx-inc for OpenFaaS
openfaas                Install openfaas
openfaas-ingress        Install openfaas ingress with TLS
openfaas-loki           Install Loki-OpenFaaS and Configure Loki logs provider for OpenFaaS
osm                     Install osm
portainer               Install portainer to visualise and manage containers
postgresql              Install postgresql
redis                   Install redis
registry-creds          Install registry-creds
sealed-secrets          Install sealed-secrets
tekton                  Install Tekton pipelines and dashboard
traefik2                Install traefik2
Let’s try the Kubernetes dashboard?
arkade install kubernetes-dashboard
The installation script prints out how to use the app, and arkade info can show us the same information later too.
#To forward the dashboard to your local machine
kubectl proxy
#To get your Token for logging in
kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user-token | awk '{print $1}')
