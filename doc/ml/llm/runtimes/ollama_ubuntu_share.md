---
title: Ollama — share over LAN from Ubuntu
main_link: https://github.com/ollama/ollama/blob/main/docs/faq.md#how-do-i-configure-ollama-server
keywords: [ollama, ubuntu, lan, systemd, ollama-host, ssh-tunnel, tailscale, wireguard]
status: reviewed
---

# Ollama — share over LAN from Ubuntu

**Main link:** <https://github.com/ollama/ollama/blob/main/docs/faq.md#how-do-i-configure-ollama-server>

## Summary

A practical recipe for sharing a single Ollama instance running on
an Ubuntu box with other machines on the LAN (or, with a tunnel,
over the internet). By default Ollama binds only to `127.0.0.1:11434`;
the fix is a systemd-unit override that sets
`OLLAMA_HOST=0.0.0.0`, plus a firewall rule and — crucially —
some kind of access control, since Ollama itself ships with no
authentication. Covers the host-side service edit, the client-side
`OLLAMA_HOST` env var, and the safer alternatives (SSH tunnel,
Tailscale/WireGuard).

## Insight

The use case is the home-lab / small-office pattern: one beefy
Linux box (a workstation with a 4090, an old gaming PC, a Mac mini
acting as a server) hosts the models, and laptops/iPads/other
machines call into it. Three things to actually watch out for:

- **`systemctl edit ollama.service` is the right move**, not
  hand-editing the unit file the package installer dropped — your
  override survives package upgrades.
- **There is no auth.** Anyone who can reach `:11434` can run
  arbitrary models, exhaust the GPU, and (via custom `Modelfile`
  entries) read paths the `ollama` user can read. Treat
  `0.0.0.0` exposure on a public network the same way you'd treat
  exposing an unauthenticated Redis.
- **Prefer a tunnel for off-LAN access.** SSH local-forward
  (`ssh -L 11434:localhost:11434 user@host`) needs zero firewall
  changes and reuses your existing SSH auth. For phones / tablets
  / always-on access, Tailscale or WireGuard give you the same
  property without the manual port-forward.

The Open WebUI / LobeChat / AnythingLLM crowd all expect this kind
of remote-Ollama setup; once you have the LAN URL working, point
those frontends at `http://<ip>:11434` and you have a personal
multi-tenant ChatGPT clone.

## Similar / related topics

- Open WebUI / LobeChat — web frontends that want a remote Ollama URL.
- llama.cpp `server` — the lower-level alternative; same exposure pattern.
- Tailscale — mesh VPN; preferred way to reach Ollama from a phone.
- LM Studio — also has a "serve over LAN" toggle in newer versions.
- vLLM / TGI — production servers if you outgrow Ollama's single-request model.

## Internal links
- [[ollama]] — canonical Ollama article.
- [[providers]] — using a remote Ollama as a Goose provider.
- [[tools/security/wireguard|wireguard]] — VPN alternative to opening the port.
- [[../README|llm]] — parent LLM hub.

## Keywords

`#ollama` `#ubuntu` `#lan` `#systemd` `#ollama-host` `#ssh-tunnel` `#tailscale` `#wireguard`

## References / raw notes

By default, Ollama only listens on `127.0.0.1` (localhost), so the
crucial step is configuring the Ubuntu machine to accept remote
connections.

### 1. Configure the Ubuntu server (the "host")

Tell Ollama on Ubuntu to listen on all network interfaces, not just
localhost.

1. **Edit the systemd service:**

    ```sh
    sudo systemctl edit ollama.service
    ```

2. **Add the environment variable.** In the editor, add:

    ```ini
    [Service]
    Environment="OLLAMA_HOST=0.0.0.0"
    ```

3. **Restart Ollama:**

    ```sh
    sudo systemctl daemon-reload
    sudo systemctl restart ollama
    ```

4. **Verify listening status.** It should now listen on
   `0.0.0.0:11434` or `:::11434`:

    ```sh
    ss -antp | grep :11434
    ```

5. **Configure the firewall.** If `ufw` is enabled:

    ```sh
    sudo ufw allow 11434/tcp
    ```

### 2. Connect from the local machine (the "client")

- **Via CLI** — set `OLLAMA_HOST` to the server's IP:

    ```sh
    export OLLAMA_HOST=<ubuntu-server-ip>:11434
    ollama run <model_name>
    ```

- **Via GUI / apps (e.g. Msty, Open WebUI, LobeChat):** create a new
  "Remote" provider with URL `http://<ubuntu-server-ip>:11434`.

### Important considerations

- **Security.** Ollama has no authentication. Anyone on the LAN
  can use the models and command the server once the port is open.
- **Safer alternatives.** For internet access, prefer **SSH
  tunnelling** over opening the port:

    ```sh
    ssh -L 11434:localhost:11434 user@<ubuntu-ip>
    ```

- **VPN.** Use Tailscale or WireGuard ([[tools/security/wireguard|wireguard]])
  to put the Ollama box on a private mesh and skip the public-IP
  problem entirely.

#ollama #ubuntu #LLM #ssh #tunneling
