# tesla-fleet-key

Public hosting for a **Tesla Fleet API third-party developer app** — the
registered domain is **`srp.kuju.email`** (Allowed Origin + Redirect URI in the
Tesla developer portal). Served via **GitHub Pages**.

## What it serves

- `.well-known/appspecific/com.tesla.3p.public-key.pem` — the app's **public**
  key, fetched by Tesla to register the app for *signed* vehicle commands. Safe
  to publish; the private signing key is NOT here.
  → `https://srp.kuju.email/.well-known/appspecific/com.tesla.3p.public-key.pem`
- `access.html` — the OAuth **redirect/callback** page, served at `/access`.
  After you authorize the app, Tesla sends the browser here with `?code=…`;
  this **client-side** page reads the code (and `state`) out of the URL and
  shows it for copy-paste, with an error view if Tesla returns `?error=`. It
  holds **no secrets** and transmits nothing — exchange the code for tokens
  yourself from a trusted environment using your client secret.
  → `https://srp.kuju.email/access`

## Plumbing

- `.nojekyll` — **required.** GitHub Pages runs Jekyll by default, which excludes
  any path starting with `.` (like `.well-known`). This empty file disables
  Jekyll so the dotfolder publishes verbatim.
- `CNAME` — binds the custom domain `srp.kuju.email` (a subdomain CNAME'd to
  `macole16.github.io`, DNS-only / unproxied on Cloudflare — Tesla requires a
  reachable, non-proxied HTTPS origin).

## History

Previously served by a hand-run Caddy `file-server` container on the `ns1`
nameserver (`srp.kuju.email` → ns1 public IP). Moved to GitHub Pages to get it
off that infrastructure — it is a personal integration, not part of the Kaimoku
fleet. Same public URL, so no Tesla developer-portal re-registration was needed.

## Updating the key

Replace the `.pem` file and push to `main`; GitHub Pages redeploys automatically.
Only ever the **public** key belongs here.
