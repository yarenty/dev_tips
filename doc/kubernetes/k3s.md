---
title: "K3s: lightweight Kubernetes for edge, IoT, and homelab"
main_link: https://k3s.io/
keywords: [k3s, kubernetes, lightweight, edge, iot, raspberry-pi, k3sup, arkade, arm, homelab, openfaas]
status: reviewed
---

# K3s: lightweight Kubernetes for edge, IoT, and homelab

**Main link:** <https://k3s.io/>

Repo: <https://github.com/k3s-io/k3s> · CNCF: <https://landscape.cncf.io/?selected=k-3s>

## Summary

[K3s](https://k3s.io/) is a CNCF-graduated, [Rancher](https://www.rancher.com/)-built distribution of Kubernetes packaged as **a single ~40 MB binary** with embedded SQLite (or external etcd / Postgres / MySQL for HA), [Traefik](https://traefik.io/) as the default ingress controller, [Flannel](https://github.com/flannel-io/flannel) for the CNI, and `containerd` for the runtime. It's API-compatible with upstream Kubernetes and CNCF-certified, but tunes everything for resource-constrained nodes (Raspberry Pi, NVIDIA Jetson, AWS A1, [Oracle Always Free Ampere](../cloud/oracle_free_tier.md)) by stripping legacy in-tree drivers, alpha features, and the cloud-controller manager. Server memory footprint is around **500 MB** vs. ~2 GB for `kubeadm`-installed upstream Kubernetes.

## Insight

Reach for K3s when:

- You're running Kubernetes on **ARM hardware** (Raspberry Pi 4/5, Jetson, AWS Graviton, Oracle Ampere). Multi-arch images and binaries are first-class.
- You want **production-grade Kubernetes on a single VPS or homelab box** without a 7-step install, an external load balancer, or a separate etcd cluster.
- You're at the **edge** — unattended remote sites, IoT gateways, in-vehicle compute. K3s ships with auto-update support and is happy to run on flaky networks.

Don't reach for K3s when:

- You need every Kubernetes feature on day one. Some alpha APIs and the in-tree cloud providers are removed; for cutting-edge demos use upstream `kubeadm`.
- You're on a managed cloud Kubernetes (EKS / GKE / AKS) — they handle the same "I don't want to babysit etcd" problem differently.
- Your workload genuinely needs the heavyweight control plane. K3s's embedded SQLite is fine for ~100s of nodes; past that you want either external etcd (HA mode) or the "real" Kubernetes.

The ecosystem worth knowing:

- **[k3sup](https://k3sup.dev/)** ("ketchup") — install K3s onto a remote box over SSH with a single command. The right tool for "I just SSH'd into the Pi for the first time".
- **[arkade](https://github.com/alexellis/arkade)** — `apt`-style installer for ~40 Kubernetes apps (cert-manager, ingress-nginx, Grafana, Loki, Postgres, Linkerd, ...) that abstracts away whether each one ships as Helm chart, raw YAML, or CLI. Far less friction than memorising helm-repo URLs.
- **[OpenFaaS](https://www.openfaas.com/)** — serverless functions on K3s; the most-cited "look how lightweight K3s is" demo platform.
- **[inlets](https://inlets.dev/)** — tunnel that gives a homelab K3s cluster a public IP without port-forwarding. Pairs well with cert-manager + Let's Encrypt.

## Similar / related topics

- [Upstream Kubernetes](https://kubernetes.io/) (`kubeadm`-installed) — what K3s is a stripped-down distribution of.
- [k0s](https://k0sproject.io/) — Mirantis's competing single-binary distribution; similar trade-offs.
- [MicroK8s](https://microk8s.io/) — Canonical's snap-packaged equivalent; popular on Ubuntu.
- [[balena]] / [[akri]] — non-Kubernetes / device-discovery alternatives for the same edge problem.
- [[dokku]] — single-host PaaS for the same "I just want to deploy my app" use case, without Kubernetes.

## Internal links

- [[oracle_free_tier]] — natural target host: 4 OCPUs / 24 GB ARM is plenty for a real K3s cluster
- [[akri]] — device discovery on the edge nodes K3s typically runs on
- [[balena]] — non-Kubernetes alternative for the same fleet/edge problem
- [[dokku]] — non-Kubernetes alternative for the "deploy my app" use case
- [[observability/grafana|grafana]] + [[prometheus]] — the canonical observability stack you'll install on top
- [[apache]] / [[ssl]] — relevant if you'll ingress with Let's Encrypt TLS

## Keywords

`#kubernetes` `#k3s` `#lightweight` `#edge` `#iot` `#raspberry-pi` `#k3sup` `#arkade` `#arm` `#homelab` `#openfaas`

## References / raw notes

### One-liner install (single node)

```sh
curl -sfL https://get.k3s.io | sh -

# Should be Ready within ~30 s
sudo k3s kubectl get node
```

### Add a worker node

```sh
# On the server, grab the join token
sudo cat /var/lib/rancher/k3s/server/node-token

# On the agent, point at the server with that token
curl -sfL https://get.k3s.io | K3S_URL=https://<server-ip>:6443 K3S_TOKEN=<token> sh -
```

Or run agent and server manually:

```sh
# Server
sudo k3s server &
# Kubeconfig is written to /etc/rancher/k3s/k3s.yaml

# Agent (different machine)
sudo k3s agent --server https://<server-ip>:6443 --token <NODE_TOKEN>
```

### Recipe: K3s on a Raspberry Pi 4 cluster (condensed from Alex Ellis's [walkthrough](https://alexellisuk.medium.com/walk-through-install-kubernetes-to-your-raspberry-pi-in-15-minutes-84a8492dc95a))

**Bill of materials** (per node)

- Raspberry Pi 4, 2 GB or 4 GB RAM (4 GB recommended if you'll run Grafana / Loki on the cluster).
- 32 GB+ SD card. Kubernetes writes to disk a lot — prefer multiple smaller cards over one giant one (cheaper to replace when one wears out).
- The official Raspberry Pi power supply. Don't cheap out, you'll buy twice.
- Build images on your laptop with [Docker Buildx](https://docs.docker.com/buildx/working-with-buildx/), not on the Pi.

**Initial Pi setup**

1. Flash Raspberry Pi OS Lite to the SD card via [balenaEtcher](https://etcher.balena.io/).
2. Before booting, create an empty `ssh` file in the boot partition (enables SSH on first boot).
3. Boot, find the Pi (`raspi.local` if mDNS works, otherwise `nmap -sP 192.168.0.0/24`).
4. `passwd pi` and `sudo raspi-config` → set memory split to 16 MB so all RAM goes to Kubernetes.
5. **Critical for K3s on Pi**: append to `/boot/cmdline.txt` (single line, no newlines):

   ```text
   cgroup_enable=cpuset cgroup_memory=1 cgroup_enable=memory
   ```

6. Reboot.

**Bootstrap K3s from your laptop**

```sh
# On your laptop — install arkade, then k3sup + kubectl through it
curl -sSL https://get.arkade.dev | sudo sh
arkade get kubectl
arkade get k3sup

# Copy your SSH key to the Pi if you haven't already
ssh-copy-id pi@raspberrypi.local

# Install K3s onto the Pi from your laptop, over SSH
export IP="192.168.0.201"     # `ifconfig` on the Pi
k3sup install --ip $IP --user pi

# k3sup writes a kubeconfig into the cwd
export KUBECONFIG=$PWD/kubeconfig
kubectl get node -o wide
```

To add more Pis, use `k3sup join --ip <new-pi-ip> --server-ip $IP --user pi`.

**Sanity-check the cluster**

```sh
kubectl top node                    # needs metrics-server (ships by default in K3s)
kubectl top pod --all-namespaces
```

### Installing common apps with `arkade`

`arkade install --help` lists ~40 apps. The ones I reach for first:

```sh
arkade install ingress-nginx          # or use the bundled Traefik
arkade install cert-manager           # TLS via Let's Encrypt
arkade install kubernetes-dashboard   # the official web UI
arkade install grafana                # see [[observability/grafana|grafana]]
arkade install loki                   # logs aggregation
arkade install postgresql             # quick DB if you need one
arkade install openfaas               # serverless functions on K3s
```

### OpenFaaS quick-start (the canonical K3s demo)

```sh
arkade get faas-cli
arkade install openfaas

# Log in (URL + password are printed by the install above)
PASSWORD=$(kubectl get secret -n openfaas basic-auth \
  -o jsonpath="{.data.basic-auth-password}" | base64 --decode)
export OPENFAAS_URL=http://<pi-ip>:31112
echo -n $PASSWORD | faas-cli login --username admin --password-stdin

# Deploy a stock function from the store (note the platform tag for ARM)
faas-cli store list --platform armhf
faas-cli store deploy figlet --platform armhf
faas-cli list
```

A minimal Go function (build-and-deploy from a separate Pi or x86 box with Docker):

```go
// my-api/handler.go
package function

import (
    "net/http"
    handler "github.com/openfaas-incubator/go-function-sdk"
)

func Handle(req handler.Request) (handler.Response, error) {
    return handler.Response{
        Body:       []byte(`Run k3s on your RPi!`),
        StatusCode: http.StatusOK,
    }, nil
}
```

```sh
faas-cli template store pull golang-http
faas-cli new --lang golang-http --prefix=$DOCKER_USER my-api
faas-cli publish -f my-api.yml --platforms linux/arm/v7  # or linux/arm64 for 64-bit Pi OS
faas-cli deploy my-api
faas-cli invoke my-api
```

### Public IP without port-forwarding ([inlets-operator](https://docs.inlets.dev/reference/inlets-operator/))

`inlets-operator` exposes K3s `Service` resources of type `LoadBalancer` as public IPs by automatically renting cheap exit-node VMs (DigitalOcean, Hetzner, AWS, ...) and tunneling back to your homelab. Pair with `cert-manager` + Let's Encrypt for free TLS on `example.com`.

### High availability and going further

- [k3sup HA mode](https://k3sup.dev/) — multiple servers behind external etcd or Postgres.
- [Bare-metal Kubernetes with K3s (Alex Ellis)](https://blog.alexellis.io/bare-metal-kubernetes-with-k3s/)
- [Set up your K3s Cluster for High Availability on DigitalOcean (Rancher)](https://rancher.com/blog/2020/k3s-high-availability)
- For a homelab, consider netbooting the Pis (boot directly off the network) so you're not babysitting SD cards.

### GitOps on top

If you'll have more than ~5 manifests in flight, switch from `kubectl apply` to GitOps:

- [Argo CD](https://argo-cd.readthedocs.io/) — pull-based, dashboard-driven; the dominant pick.
- [Flux](https://fluxcd.io/) — pull-based, more lightweight, CRD-heavy.

### Learning resources

- **CNCF Landscape** — <https://landscape.cncf.io/>
- **Kubernetes docs + tutorials** — <https://kubernetes.io/docs/home/>, <https://kubernetes.io/docs/tutorials/>
- **edX: Introduction to Kubernetes on Edge with K3s (LFS156x)** — <https://learning.edx.org/course/course-v1:LinuxFoundationX+LFS156x+2T2021/home>
- **edX: Introduction to Serverless on Kubernetes (LFS157x)** — <https://www.edx.org/course/introduction-to-serverless-on-kubernetes>
- **Linux Foundation certifications** — CKA / CKAD / CKS for the formal-credential route.

### Smaller cousin: faasd

If you want OpenFaaS without Kubernetes at all (single-host, even tinier than K3s), look at [faasd](https://github.com/openfaas/faasd) — OpenFaaS on `containerd` directly, no Kubernetes runtime. Good for very resource-constrained edge devices where K3s is still too heavy.

### Hardware sources for Pi-cluster builders

- [Raspberry Pi Foundation](https://www.raspberrypi.org/products/) — the boards themselves.
- [BitScope Cluster Blade](https://www.bitscope.com/blog/JK/?p=JK38B) — industrial-style cluster cases.
- [Pimoroni](https://pimoroni.com/) — sensors and HATs.
- KubeCon talk — *The Past, Present, and Future of Kubernetes on Raspberry Pi*: <https://www.youtube.com/watch?v=jfUpF40--60>
