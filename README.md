# vpn-switcher

A tiny web page for moving a [gluetun](https://github.com/qdm12/gluetun) VPN tunnel to a different
server country from the browser. It is nginx serving one HTML file, with nginx proxying three
gluetun control-server routes and adding the API key — the key never reaches the browser.

> **Built with AI.** This was written with the help of an AI assistant. Use it at your own risk —
> no warranty, and I'm not responsible for anything it does to your setup. Read it before you run it.

## What it needs
- gluetun with its control server (port 8000) reachable from this container on a shared Docker network
- a gluetun auth role allowing only `GET /v1/publicip/ip`, `GET /v1/vpn/status`, `PUT /v1/vpn/settings`
  (example: `gluetun-auth.example.toml`)

## Setup
1. `cp nginx.conf.example nginx.conf`, then replace `__GLUETUN_API_KEY__` with your key and
   `__PROXY_IP__` with the address allowed to open the page (e.g. your reverse proxy); change
   `gluetun:8000` if your gluetun container has another name. Keep `nginx.conf` at mode 600 — it holds the key
   (it is in `.gitignore`).
2. Put the same key in your gluetun auth config (`gluetun-auth.example.toml`).
3. Add the service from `docker-compose.example.yml` to your compose file and `docker compose up -d vpn-switcher`.
4. Don't expose it to the internet. Put it behind your reverse proxy with authentication, or keep it LAN-only.

## Licence
MIT — see `LICENSE`.
