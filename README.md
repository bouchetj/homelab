# 🏠 Homelab
> I built a self-hosted homelab with a pre-owned HP EliteDesk 800 G3 to fully own the stack that stores my files, photos, and network. This repository is the logbook of that build: infrastructure-as-code for every container, network, and automation task that keeps my personal cloud online.

## 🔩 Hardware Loadout
- 🖥️ **HP EliteDesk 800 G3 Desktop** for quiet, efficient always-on duty
- 💾 **8TB HDD add-on** to extend storage for media, backups, and photo archives

## 🐳 Platform Principles
- 📦 Everything ships as Docker containers with `docker compose`, making rebuilds painless
- 🧾 Config lives in Git; secrets stay in local `.env` files that never leave the box
- 🔐 Public DNS points at `jubslab.org`, but services stay gated behind my self-hosted VPN
- 🧭 Reverse proxy + Cloudflare tunnel provide TLS, logging, and a consistent edge

## 🌐 Access Model
`*.jubslab.org` resolves to the homelab, but the door only opens if you arrive through my personal VPN. Once on the VPN, the reverse proxy (NGINX) fans traffic to the right stack, and Cloudflare keeps certificates renewed and DNS tidy without exposing anything directly to the wider internet.

## 📦 Service Catalog
| Service | What it does | Compose file |
| --- | --- | --- |
| 📁 **Nextcloud** | My personal cloud suite for files, calendars, and collaboration. | `storage/nextcloud/docker-compose.yml` |
| 🖼️ **Immich** | Self-hosted photo and video backup with AI-powered search. | `storage/immich/docker-compose.yml` |
| 🚫 **Pi-hole** | Network-wide DNS-based ad and tracker blocking. | `network/pihole/docker-compose.yml` |
| 🛡️ **NGINX Reverse Proxy** | Terminates TLS, applies request routing, and bridges VPN traffic to services. | `core/reverse-proxy/docker-compose.yml` |
| ☁️ **Cloudflared Tunnel** | Maintains secure outbound tunnel + DNS updates for the `jubslab.org` edge. | `core/cloudflare/docker-compose.yml` |
| 📊 **Glance** | Dashboard that monitors container health and quick links for daily ops. | `ops/glance/docker-compose.yml` |

## 🗂️ Repository Map
- `core/` — edge services that front everything else (reverse proxy, Cloudflare tunnel)
- `network/` — infrastructure that shapes inbound/outbound traffic (Pi-hole, VPN bits)
- `storage/` — data-heavy apps like Nextcloud and Immich with dedicated volumes
- `ops/` — observability and tooling to keep the lab usable day to day
- `crispr-app/` — self-hosted version of the CRISPR app

## 🚀 Bring It Online
1. Create the shared Docker networks once: 
   ```bash
   docker network create walle-network
   docker network create edge
   ```
2. Create `.env` files next to each `docker-compose.yml` with your secrets, domain names, and storage paths.
3. From any service folder, launch the stack with `docker compose up -d`.

## 🔮 What’s Next?
More automations, smarter monitoring, and maybe a few new services...
