---
title: Signing 3rd-party Apache modules on macOS Big Sur+
main_link: https://blog.phusion.nl/2020/12/22/future_of_macos_apache_modules/
keywords: [apache, macos, code-signing, certificate-authority, big-sur, mod-passenger, keychain]
status: reviewed
review_date: 2026/05/03
---

# Signing 3rd-party Apache modules on macOS Big Sur+

**Main link:** <https://blog.phusion.nl/2020/12/22/future_of_macos_apache_modules/>

## Summary

A walk-through (Camden Narzt, Phusion) for **future-proofing 3rd-party Apache modules** (e.g. `mod_passenger`, third-party `mod_php`) on modern macOS. Starting with Big Sur, Apple's bundled Apache logs `AH06665: No code signing authority for module …` — a deprecation warning that will eventually become an error. The fix is to **create your own self-signed Code-Signing CA in Keychain Access**, sign the module with `codesign`, and tell Apache the authority to trust via `LoadModule … "Common Name"`.

## Summary in one sentence

Make your own CA, sign the `.so`, add the CA's Common Name to the `LoadModule` directive — done.

## Insight

You'll need this whenever you want to use a third-party Apache module on macOS without buying an Apple Developer Program account. The recipe works for any `.so` Apache loads — Passenger, mod_php, custom mods. The clever bit is the new `"Common Name"` field at the end of the `LoadModule` directive (added in Big Sur) which lets Apache accept any signing authority *that's trusted on the local machine*, not just Apple-approved ones. Once your CA is in the user keychain marked Always Trust, your self-signed mods load with `AH06662: Allowing module loading process to continue …` instead of the deprecation warning.

The Keychain Access workflow is finicky and easy to get wrong; follow the steps in order and don't use the shortcuts in the Certificate Assistant menu (they trigger weird "wrong key" bugs). Save the CA — you'll need it again every macOS upgrade or when a new module ships.

## Similar / related topics

- [[apache]] — `apachectl` cheat-sheet for the macOS system Apache.
- **`codesign(1)` man page** — Apple's CLI for code signing.
- **Apple Developer Program** — paid alternative; gets you a real Team ID so you don't need this hack.
- **Homebrew Apache** (`brew install httpd`) — alternative path: a separate Apache install that doesn't enforce code-signing.
- Phusion Passenger docs: <https://www.phusionpassenger.com/library/install/apache/>

## Internal links

- [[apache]] — System Apache cheat-sheet you'll need alongside this.
- [[ssl]] — Companion HTTPS-on-Apache note.

## Keywords

`#apache` `#macos` `#code-signing` `#certificate-authority` `#big-sur` `#mod-passenger` `#keychain`

## References / raw notes

- Original article (Camden Narzt, Phusion, Dec 2020): <https://blog.phusion.nl/2020/12/22/future_of_macos_apache_modules/>
- `codesign(1)`: `man codesign`
- Apache `LoadModule` docs: <https://httpd.apache.org/docs/2.4/mod/mod_so.html#loadmodule>

### Background — the chain of macOS changes

1. **macOS 10.14 Mojave** introduced Library Validation for Apache modules — only Apple-signed or paid-developer-signed `.so` files would load. Broke every 3rd-party module.
2. **10.14.4** — Apple reverted the change after community pushback.
3. **Big Sur (11)** — re-introduced as a *deprecation warning*: `AH06665: No code signing authority for module …` Modules still load but Apple is signalling future enforcement.
4. **The fix** — Apache 2.4 added an optional `"Common Name"` argument on `LoadModule` so Apple can let you specify your own trusted authority.

### Two error variants you may have seen

```log
# (1) Unsigned module:
httpd: ... mapped file has no cdhash, completely unsigned? Code has to be at least ad-hoc signed.

# (2) Ad-hoc signed but no Team ID:
httpd: ... mapped file has no Team ID and is not a platform binary
       (signed with custom identity or adhoc?)
```

The walk-through below resolves both.

### Step-by-step recipe

#### A. Create a self-signed Code-Signing Certificate Authority

Open **Keychain Access.app**, then `Keychain Access → Certificate Assistant → Open…` (use this menu path; do NOT use the more-direct shortcuts — they trigger contextual bugs).

1. Choose **Create a Certificate Authority (CA)**.
2. Pick a **name** for your CA (e.g. `My Code Signing CA`).
3. Set your **email** (any address you control).
4. Set **User Certificate** dropdown → **Code Signing**.
5. Click through the rest of the wizard, accepting defaults except:
   - On the *Capabilities* page, ensure **Code Signing** is checked.
   - On the *Critical Extensions* page, ensure Code Signing is the only checked item AND tick **This extension is critical**.
6. Finish. Your CA appears in the main Keychain Access window.
7. **Right-click the CA → Get Info → Trust → "When using this certificate" = Always Trust**. Authenticate when prompted.

#### B. Issue a code-signing leaf certificate from that CA

1. Open the Certificate Assistant again.
2. Choose **Create a certificate for yourself**.
3. Pick a name for the leaf cert (e.g. `Yarenty Code Signing`).
4. Set **Identity Type** = `Leaf`, **Certificate Type** = `Code Signing`.
5. Click **Create** → **Continue** → **Create** (uses your CA from step A to sign).
6. **Note the Common Name** of the leaf cert — you'll need it twice below.

#### C. Sign the Apache module

```shell
codesign \
  -s "Yarenty Code Signing" \
  --keychain ~/Library/Keychains/login.keychain-db \
  /usr/local/opt/passenger/libexec/buildout/apache2/mod_passenger.so
```

Substitute the path to whichever module you're signing.

#### D. Tell Apache to trust this authority for that module

Edit `/etc/apache2/other/passenger.conf` (or wherever the `LoadModule` lives):

```apache
LoadModule passenger_module \
    /usr/local/opt/passenger/libexec/buildout/apache2/mod_passenger.so \
    "Yarenty Code Signing"
```

Validate config and reload:

```shell
sudo apachectl -t
sudo apachectl restart
```

You should now see in the error log:

```log
AH06662: Allowing module loading process to continue for module at
/usr/local/opt/passenger/libexec/buildout/apache2/mod_passenger.so
because module signature matches authority "Yarenty Code Signing"
specified in LoadModule directive
```

### Why Phusion can't sign modules for you (their answer)

- Sub-trusting their cert chain on your Mac is your responsibility — they don't want to be a root CA on user systems.
- Most Passenger installs come via Homebrew, which doesn't currently distribute a code-signing certificate.
- Their build box doesn't currently sign mods either.
