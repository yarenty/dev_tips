---
title: "Oracle Cloud Always Free Tier (especially the Ampere A1 ARM instance)"
main_link: https://www.oracle.com/cloud/free/
keywords: [oracle-free-tier, oci, ampere-a1, arm, free-vps, autonomous-database, vcn]
status: reviewed
---

# Oracle Cloud Always Free Tier (especially the Ampere A1 ARM instance)

**Main link:** <https://www.oracle.com/cloud/free/>

Always-free resources reference: <https://docs.oracle.com/en-us/iaas/Content/FreeTier/freetier_topic-Always_Free_Resources.htm>

## Summary

Oracle Cloud Infrastructure (OCI) ships an "Always Free" tier that is, by some distance, the most generous free-as-in-beer compute offering of any major cloud — most notably **4 OCPUs and 24 GB of RAM** worth of `VM.Standard.A1.Flex` (Ampere ARM) compute, which you can carve up into one fat VM or several smaller ones. On top of the compute you get 200 GB of block storage, 20 GB of object storage, two Autonomous Database instances, an Oracle NoSQL database with 25 GB per table, and a Vault with 150 secrets — all permanently, no expiry. It's the closest thing on the market to "free hobby VPS forever".

## Insight

Reach for OCI Always Free when you want a long-running VPS for a hobby project — a Mastodon instance, a small game server, a personal CI runner, a self-hosted Bitwarden, a place to park a `cron` you don't want to lose. The Ampere A1 instances are *real* ARMv8 servers (not throttled "burst" credits like AWS t-class), so the resource quota is the actual usable amount.

Three real downsides worth knowing before you commit:

1. **Capacity scarcity.** The A1 ARM instance has been famously hard to provision in some regions for years — Oracle let demand outstrip supply, and you can spend hours retrying with `Out of capacity for shape VM.Standard.A1.Flex` errors. Use a different home region (e.g. one of the smaller European or APAC regions) and/or a script that retries until capacity appears. Or just use `VM.Standard.E2.1.Micro` (1 OCPU AMD, ~1 GB RAM) which is also Always Free and almost always available.
2. **OCI's IAM and console.** Compared to AWS or GCP, OCI's IAM model (compartments, policies, dynamic groups) and the web console are noticeably clunkier. Plan to spend an afternoon learning the vocabulary even if you've used other clouds.
3. **Ingress is not open by default.** A fresh VM has *no* inbound ports open at the VCN level — you have to add a security-list ingress rule explicitly (see the recipe below). The Ubuntu firewall is permissive but the VCN's isn't.

Migration risk: Oracle has historically been more willing than AWS/GCP to suspend or reclaim under-utilised free-tier accounts. Treat this as "free with strings", not "no commitment" — keep your data backed up off-platform.

## Similar / related topics

- [[apache]] — the Apache + PHP install steps below; this article only covers the OCI-specific glue.
- [[k3s]] — what to install on the 4-OCPU / 24-GB instance if you want a real Kubernetes for hobby workloads.
- [Hetzner Cloud](https://www.hetzner.com/cloud) — the cheapest *paid* alternative; ~€4/month gets you a real KVM instance with no capacity games.
- [Fly.io](https://fly.io/) — generous free credits for small persistent VMs; better DX than OCI.
- [AWS Free Tier](https://aws.amazon.com/free/) / [GCP Free Tier](https://cloud.google.com/free) — both expire after 12 months for the meaningful resources; OCI's are *always* free.

## Internal links

- [[apache]] — Apache + `apachectl` cheat-sheet referenced from the install recipe below
- [[k3s]] — lightweight Kubernetes; pairs well with the 4-OCPU/24-GB ARM instance
- [[ssl]] — TLS / Let's Encrypt for whatever you put on port 80

## Keywords

`#cloud` `#oci` `#oracle-cloud` `#ampere-a1` `#arm` `#free-vps` `#always-free` `#vcn` `#autonomous-database`

## References / raw notes

### What you actually get (per the [Always Free reference](https://docs.oracle.com/en-us/iaas/Content/FreeTier/freetier_topic-Always_Free_Resources.htm))

| Resource | Always Free quota |
|----------|-------------------|
| **Ampere A1 ARM compute** (`VM.Standard.A1.Flex`) | **4 OCPUs + 24 GB RAM total**, carve into 1–4 instances. Linux only (Oracle Linux Cloud Developer / Oracle Linux / Ubuntu). |
| **AMD compute** (`VM.Standard.E2.1.Micro`) | 2 instances, each 1 OCPU + 1 GB RAM. Almost always available (use as fallback). |
| **Block storage** | 200 GB total across boot + block volumes; 5 volume backups. New compute instance auto-gets a 50 GB boot volume. |
| **Object Storage** | 20 GB total. |
| **Vault** | 20 HSM-protected master-key versions + 150 software secrets. Software-protected master keys are unlimited. |
| **Autonomous Database** | 2 instances; supports OLTP / DW / APEX / JSON workloads. |
| **Oracle NoSQL** | 133 M reads/month, 133 M writes/month, 3 tables × 25 GB. |
| **Resource Manager** (Terraform) | Workspace + state storage included. |
| **Networking** | Bandwidth + VNICs scale with OCPU count; outbound transfer is generous. |

`VM.Standard.A1.Flex` is genuinely flexible: you tell the create-instance wizard "give me N OCPUs and M GB RAM" and it slices the quota accordingly. Common picks: `4 OCPU + 24 GB` (one fat box) or `2 + 12` × 2 (two medium boxes).

### Open inbound port 80 on the VCN (the most-missed step)

A fresh OCI VM is firewalled at the VCN security-list layer, so you can't reach it on port 80/443 even though the VM itself is happy to serve. Recipe:

1. **Console** → **Networking** → **Virtual Cloud Networks** → click your VCN.
2. Click `<your-subnet-name>` (the public subnet auto-created by the instance wizard).
3. Click **Default Security List**.
4. **Add Ingress Rules** → fill in:

   | Field | Value |
   |-------|-------|
   | Stateless | ☑ (off is fine too; stateless skips the connection-tracking table) |
   | Source Type | CIDR |
   | Source CIDR | `0.0.0.0/0` |
   | IP Protocol | TCP |
   | Source port range | (leave blank) |
   | Destination Port Range | `80` |
   | Description | `Allow HTTP` |

5. Repeat for `443` if you'll terminate TLS on the box (you almost certainly should — see [[ssl]]).

The Ubuntu host firewall (`ufw`) is *off* by default on Oracle's Ubuntu image, but `iptables` rules from `iptables-services` are pre-loaded and *do* drop port 80. So after opening the VCN ingress, also run on the box:

```sh
sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 80 -j ACCEPT
sudo netfilter-persistent save
```

(Repeat with `--dport 443` for HTTPS.)

### Apache + PHP on the Ampere ARM instance

The OCI tutorial — [Apache and PHP on Ubuntu](https://docs.oracle.com/en-us/iaas/developer-tutorials/tutorials/apache-on-ubuntu/01oci-ubuntu-apache-summary.htm) — is just the standard Ubuntu Apache install with the VCN bit above. Recipe condensed:

```sh
# SSH in (key was attached at instance-create time)
ssh -i ~/.ssh/oci_key ubuntu@<your-public-ip>

# Apache
sudo apt update
sudo apt -y install apache2
sudo systemctl restart apache2

# Open port 80 in iptables (see VCN section above first)
sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 80 -j ACCEPT
sudo netfilter-persistent save

# Sanity check
curl localhost          # should show the Apache default page

# PHP
sudo apt -y install php libapache2-mod-php
php -v
sudo systemctl restart apache2

# Smoke-test PHP
echo '<?php phpinfo(); ?>' | sudo tee /var/www/html/info.php
# visit http://<public-ip>/info.php in a browser
sudo rm /var/www/html/info.php   # delete after testing — phpinfo leaks env
```

For everything beyond "is Apache running" — vhost setup, `apachectl` workflow, `ssl.conf`, troubleshooting — see the dedicated [[apache]] article. For Let's Encrypt TLS automation see [[ssl]].
