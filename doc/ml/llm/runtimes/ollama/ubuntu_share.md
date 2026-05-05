---
title: Ubuntu Share
main_link: 
keywords: [ubuntu-share, configure, ubuntu, share, internet, cli]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# Ubuntu Share

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[wireguard]] — Wireguard _(score 18.0)_
- [[applications]] — Deskreen _(score 8.9)_
- [[tools/shell/tmux|tmux]] — Tmux _(score 8.9)_
- [[programming/rust/misc/drogue|drogue]] — Drogue _(score 6.0)_
- [[sniffnet]] — sniffnet _(score 6.0)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#ubuntu-share` `#ollama` `#runtimes` `#llm` `#ubuntu` `#machine` `#configure` `#server`

## TODO

- No `main_link` could be auto-detected. Add the canonical URL (project homepage / repo / paper) to the frontmatter.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->


By default, Ollama only listens on `127.0.0.1` (localhost), so the crucial step is configuring the Ubuntu machine to accept remote connections. 

Here is how to set it up:

1. Configure the Ubuntu Server (The "Host") 

You must tell Ollama on Ubuntu to listen on all network interfaces, not just localhost. 

1. **Edit the systemd service:** Run `sudo systemctl edit ollama.service`.
2. **Add Environment Variable:** In the editor, add the following lines to allow external connections:
    
    ini
    
    ```
    [Service]
    Environment="OLLAMA_HOST=0.0.0.0"
    ```
    
3. **Restart Ollama:** Reload systemd and restart the service:
    
    bash
    
    ```
    sudo systemctl daemon-reload
    sudo systemctl restart ollama
    ```
    
4. **Verify Listening Status:** Run `ss -antp | grep :11434` to ensure it is now listening on `0.0.0.0:11434` or `:::11434`.
5. **Configure Firewall:** If you have `ufw` enabled, allow the port:
    
    bash
    
    ```
    sudo ufw allow 11434/tcp
    ```
    
     

6. Connect from Local Machine (The "Client") 

On your local computer, you can now connect to the Ubuntu machine. 

- **Via CLI:** Set the `OLLAMA_HOST` environment variable to the Ubuntu server's IP address:
    
    bash
    
    ```
    export OLLAMA_HOST=<ubuntu-server-ip>:11434
    ollama run <model_name>
    ```
    
- **Via GUI/Apps (e.g., Msty):** In your GUI tool, create a new "Remote" provider and set the URL to `http://<ubuntu-server-ip>:11434`. 

Important Considerations

- **Security:** By default, Ollama has no authentication. Anyone on your local network can use the models and command the server if you open it up.
- **Alternatives for Security:** If you need to access it over the internet, it is recommended to use **SSH Tunneling** (e.g., `ssh -L 11434:localhost:11434 user@<ubuntu-ip>`) instead of opening the port publicly.
- **VPN:** For safer access, use a VPN like Tailscale or WireGuard to connect to the Ubuntu machine.
---

#ollama #ubuntu #LLM #ssh #tunneling
