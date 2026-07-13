# tesla-fleet-key

Public hosting for a **Tesla Fleet API third-party developer public key**.

Tesla requires third-party apps to publish their EC public key at a fixed
well-known path over public HTTPS so Tesla can register the app for *signed*
vehicle commands. This repo serves exactly that one file via **GitHub Pages**:

```
https://srp.kuju.email/.well-known/appspecific/com.tesla.3p.public-key.pem
```

## How it works

- `.well-known/appspecific/com.tesla.3p.public-key.pem` — the **public** key (safe to publish; the private signing key is NOT here).
- `.nojekyll` — **required.** GitHub Pages runs Jekyll by default, which excludes any path starting with `.` (like `.well-known`). This empty file disables Jekyll so the dotfolder publishes verbatim.
- `CNAME` — binds the custom domain `srp.kuju.email` (a subdomain CNAME'd to `macole16.github.io`, DNS-only on Cloudflare).

## History

Previously served by a hand-run Caddy `file-server` container on the `ns1`
nameserver (`srp.kuju.email` → ns1 public IP). Moved to GitHub Pages to get it
off that infrastructure — it is a personal integration, not part of the Kaimoku
fleet. Same public URL, so no Tesla developer-portal re-registration was needed.

## Updating the key

Replace the `.pem` file and push to `main`; GitHub Pages redeploys automatically.
Only ever the **public** key belongs here.
