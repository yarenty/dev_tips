---
title: Once Proxying you can navigate to the below
main_link: http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login
keywords: [once-proxying-you-can-navigate-to-the-below, kubernetes, openfaas, faas, cli, username]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/kubernetes/k3s.md` by `article_split.py`. Heading: **Once Proxying you can navigate to the below**.

# Once Proxying you can navigate to the below

**Main link:** <http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#once-proxying-you-can-navigate-to-the-below` `#kubernetes` `#openfaas` `#faas` `#cli` `#username`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Once Proxying you can navigate to the below
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login
Paste in your token

Now enjoy the dashboard:


Let’s install another popular application, openfaas. OpenFaaS gives us a simple way to deploy functions and microservices to Kubernetes with built-in auto-scaling.
arkade get faas-cli
arkade install openfaas
Log in using the post-installation information.
The IP of my RPi is 192.168.0.201, so I can access OpenFaaS using a NodePort of 31112.
PASSWORD=$(kubectl get secret -n openfaas basic-auth -o jsonpath="{.data.basic-auth-password}" | base64 --decode; echo)
export OPENFAAS_URL=http://192.168.0.201:31112
echo -n $PASSWORD | faas-cli login --username admin --password-stdin
faas-cli store list --platform armhf
faas-cli store deploy figlet --platform armhf
faas-cli list
Now open the OpenFaaS UI and check your figlet function using http://192.168.0.201:31112 or the equivalent.


You can also build your own functions with Python, Go, JavaScript and many other languages.
If you have a Docker Hub login, then you can try the following, but you’ll need to run it on a separate Raspberry Pi, with docker installed (curl -sSL https://get.docker.com | sudo sh)
export USERNAME=alexellis2
docker login -u $USERNAME
faas-cli template store pull golang-http
faas-cli new --lang golang-http --prefix=$USERNAME my-api
