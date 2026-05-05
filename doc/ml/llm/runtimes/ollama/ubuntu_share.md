
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

