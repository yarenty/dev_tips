---
title: Apache HTTPD on macOS — quick commands
main_link: https://httpd.apache.org/
keywords: [apache, apachectl, httpd, macos, web-server, localhost]
status: reviewed
review_date: 2026/05/03
---

# Apache HTTPD on macOS — quick commands

**Main link:** <https://httpd.apache.org/>

## Summary

Cheat-sheet for the **Apache HTTPD web server** that ships pre-installed on macOS. Macs have shipped with Apache for ~20 years (now in `/usr/sbin/httpd` with config in `/etc/apache2/`); it's the fastest way to get a local web server running for testing, signing experiments, or PHP development without installing anything. Controlled with `apachectl`.

## Insight

Use the system Apache when you just need *a localhost web server* and don't want a Homebrew install or a Docker container. It's invaluable for testing things like SSL renewal, Apache module loading (see [[signing_mac_libs]]), and old-school PHP/CGI experiments. **Don't** use it for actual development of new web apps — Apache is fine but heavyweight; for that reach for `caddy`, `python -m http.server`, or your framework's bundled dev server. The system Apache is also the one that mod-signing changes in Big Sur affect, so if you're hitting "code signature not valid" loading 3rd-party modules, you're in [[signing_mac_libs]] territory.

The default doc-root on macOS is `/Library/WebServer/Documents/`. The default config is `/etc/apache2/httpd.conf`.

## Similar / related topics

- [[signing_mac_libs]] — How to load 3rd-party Apache modules on modern macOS (Mojave+).
- **nginx** — the most-used alternative; `brew install nginx`.
- **Caddy** — modern HTTPS-by-default web server; cleaner config than Apache.
- **`python -m http.server` / `npx serve`** — for one-off static-file serving.
- [[ssl|HTTPS on websites]] — adding HTTPS via Certbot once Apache is up.

## Internal links

- [[signing_mac_libs]] — Companion: signing 3rd-party Apache modules so macOS lets them load.
- [[ssl]] — Adding HTTPS via Certbot.

## Keywords

`#apache` `#apachectl` `#httpd` `#macos` `#web-server` `#localhost`

## References / raw notes

- Apache HTTPD project: <https://httpd.apache.org/>
- macOS Apache docs (Apple): <https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPInternational/MaintaingYourOwnLocalizedStrings/MaintaingYourOwnLocalizedStrings.html>
- Default macOS config: `/etc/apache2/httpd.conf`
- Default doc-root: `/Library/WebServer/Documents/`

### `apachectl` cheat-sheet

```shell
# Start the system Apache (needs sudo on macOS):
sudo apachectl start

# Stop:
sudo apachectl stop

# Restart (graceful):
sudo apachectl restart

# Test config without restarting:
sudo apachectl -t

# Find the version:
httpd -v

# Tail the access log:
tail -f /var/log/apache2/access_log

# Tail the error log (most useful when modules fail to load):
tail -f /var/log/apache2/error_log
```

After `apachectl start`, open <http://localhost> — by default you'll see the cheerful "It works!" page.

### Common troubleshooting

- **"AH06665: No code signing authority"** — see [[signing_mac_libs]].
- **Port 80 already in use** — likely macOS web sharing is enabled in System Settings → Sharing, or another web server is running. `sudo lsof -i :80` to find the culprit.
- **Permission denied on doc-root** — Apache runs as user `_www`; check that the doc-root is readable by it.
- **Changes to `httpd.conf` not taking effect** — you forgot `sudo apachectl restart`. `apachectl -t` first to validate syntax.
