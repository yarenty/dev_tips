---
title: "Dokku: a self-hosted, single-box mini-Heroku"
main_link: https://dokku.com/
keywords: [dokku, paas, heroku, git-push-to-deploy, self-hosted, vps, docker, hobby-projects]
status: reviewed
review_date: 2026/05/03
---

# Dokku: a self-hosted, single-box mini-Heroku

**Main link:** <https://dokku.com/>

Install (apt): <https://dokku.com/docs/getting-started/install/> · Repo: <https://github.com/dokku/dokku>

> Note: filed under `kubernetes/` in this vault because it solves the same "deploy my app somewhere" problem space, but **Dokku is not Kubernetes**. It runs Docker containers directly on a single host with its own scheduler. If you want a Kubernetes-shaped equivalent, look at [[k3s]].

## Summary

[Dokku](https://dokku.com/) is the smallest self-hosted "Platform-as-a-Service" you'll find: a couple of bash scripts and a [Herokuish](https://github.com/gliderlabs/herokuish) container that turn a fresh VPS into a `git push` target. You `git push dokku my-app main`, Dokku detects the language, builds a Heroku-compatible buildpack image, runs it, attaches a database, sets up an nginx reverse proxy, and gives you a URL — all on one box, with no Kubernetes, no helm, no YAML.

## Insight

Dokku is the right answer when you want **Heroku ergonomics on hardware you control**:

- A €4/month [Hetzner](https://www.hetzner.com/cloud) box, an [Oracle Always Free Ampere](../cloud/oracle_free_tier.md) instance, or a Raspberry Pi at home.
- A handful of hobby apps you don't want to babysit individually with `systemd` + nginx + certbot.
- The Heroku 12-factor / buildpack mental model genuinely fits how you write web apps.

It's *not* the right answer when:

- You need horizontal scaling across multiple machines. Dokku is single-host by design (there's a community plugin for multi-host but it's a different beast).
- You're already in Kubernetes-land. Use [[k3s]] instead — same bare-metal idea, CNCF-shaped tooling.
- Your app has hard non-12-factor requirements (sticky filesystem state, custom kernel modules, weird runtimes). The buildpack model will fight you.

The two killer features:

1. **Plugins for everything operational**: Postgres, Redis, MySQL, Let's Encrypt TLS, S3 backups, basic auth, scheduled jobs, log shipping. Each is a one-liner like `dokku postgres:create my-db && dokku postgres:link my-db my-app`.
2. **Heroku buildpack compatibility**: anything that runs on Heroku runs on Dokku unchanged, including Procfile semantics, environment variables, and `heroku-postbuild` hooks. So a working Heroku app moves over with zero code changes.

The catch nobody mentions until you hit it: Dokku's "deploy" is `git push`, which means **the deployer machine and the build machine are the same host**. A Rails or Webpack build that uses 6 GB of RAM during compilation will fight your running apps for memory unless you give the box enough headroom or use the optional remote-build setup.

## Similar / related topics

- [[k3s]] — Kubernetes-based alternative; more complex but scales out across nodes.
- [Caprover](https://caprover.com/) — Dokku's main "modern web UI" competitor; similar single-host PaaS shape.
- [Coolify](https://coolify.io/) — newer self-hosted Heroku-alike with a polished web UI.
- [Heroku](https://www.heroku.com/) — the original, what Dokku mimics; still a fine choice if you'll pay for not running infra.
- [Fly.io](https://fly.io/) / [Render](https://render.com/) — managed PaaS without the self-host commitment.

## Internal links

- [[k3s]] — Kubernetes-based alternative for the same "deploy my app" problem
- [[oracle_free_tier]] — natural target host: 4 OCPUs / 24 GB free is plenty for a Dokku box
- [[ssl]] — Dokku has a `letsencrypt` plugin but understanding the underlying flow helps
- [[apache]] — Dokku ships with nginx, but the reverse-proxy concepts transfer

## Keywords

`#kubernetes` `#dokku` `#paas` `#heroku` `#git-push-to-deploy` `#self-hosted` `#vps` `#docker` `#hobby-projects`

## References / raw notes

### Pitch (paraphrased from the docs)

> The smallest PaaS implementation you've ever seen. Dokku helps you build and manage the lifecycle of applications.
>
> Dokku offers many of the features seen in "Platform-as-a-Service" offerings such as Heroku. Unlike many PaaS offerings, you can host it on a cheap VPS service.
>
> Although it is well suited for hobby projects, it's important to note that Dokku also works wonderfully in production settings.

### Install on a fresh Ubuntu/Debian VPS

```sh
# As root on the VPS
wget -NP . https://dokku.com/install/v0.34.0/bootstrap.sh
sudo DOKKU_TAG=v0.34.0 bash bootstrap.sh
# After install: visit http://<server-ip>/ in a browser to set the SSH key + hostname
```

### Deploy your first app

```sh
# On the server
ssh root@<server>
dokku apps:create my-app
dokku postgres:create my-db
dokku postgres:link my-db my-app

# On your laptop, in your app's git repo
git remote add dokku dokku@<server>:my-app
git push dokku main
# Dokku builds, deploys, and prints the live URL
```

### Useful plugin recipes

```sh
# Free TLS via Let's Encrypt
dokku plugin:install https://github.com/dokku/dokku-letsencrypt.git
dokku letsencrypt:set my-app email you@example.com
dokku letsencrypt:enable my-app
dokku letsencrypt:cron-job --add

# Persistent Postgres + automatic backup to S3
dokku postgres:create my-db
dokku postgres:backup-auth my-db <AWS_KEY> <AWS_SECRET>
dokku postgres:backup-schedule my-db "0 3 * * *" my-bucket

# Map a custom domain
dokku domains:set my-app app.example.com
```

### Useful entry points

- Site: <https://dokku.com/>
- Docs: <https://dokku.com/docs/>
- Plugin index: <https://dokku.com/docs/community/plugins/>
- Repo: <https://github.com/dokku/dokku>
