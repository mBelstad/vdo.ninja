# Coolify media stack (single app)

This folder contains a single-application deployment for the media stack:

- VDO.Ninja (white-label static site)
- MediaMTX (SRT ingest, WHIP/WHEP, HLS, recordings)
- coturn (optional TURN for WebRTC)

Coolify can front this app with its built-in proxy and certificates for `*.itagenten.no`.

## Files

- `docker-compose.yml`: services and ports
- `Dockerfile.vdoninja`: builds a static web server (Caddy) serving this repo
- `Caddyfile.vdoninja`: static file server (Coolify handles HTTPS)
- `mediamtx.yml`: MediaMTX config (SRT/WHIP/WHEP/HLS/recordings)
- `turnserver.conf.sample`: sample TURN config

## Domains

- Studio (VDO.Ninja): `studio.itagenten.no` (or any subdomain under `*.itagenten.no`)
- Media (MediaMTX endpoints): `media.itagenten.no`

In Coolify:

1) Create one app from this folder (docker compose template).
2) Attach both domains to the app and route:
   - `studio.itagenten.no` → service `vdoninja` (port 80)
   - `media.itagenten.no` → service `mediamtx` (port 8888)
3) Enable SSL for both with Let’s Encrypt.

## MediaMTX usage

- SRT ingest (caller mode from cameras):
  - URL: `srt://media.itagenten.no:8890?streamid=cam1` (cam1..cam5)
    - If `publishPass` is enabled for a path, add `&pass=THE_KEY`
- WHIP publish from VDO.Ninja mixer:
  - In VDO.Ninja add `&whippush=https://media.itagenten.no/whip/mixer` (or `&mediamtx=media.itagenten.no:8888`)
- WHEP playback:
  - `https://media.itagenten.no/whep/cam1`
- HLS (2–6s latency for viewers):
  - `https://media.itagenten.no/hls/cam1/index.m3u8`

## Recording

- MediaMTX records to `/recordings` (30 days retention). Volume `mediamtx-recordings` persists data.

## TURN (optional)

- coturn listens on UDP 3478; point WebRTC clients if needed.
- To add TLS on 443, provide certs and adjust `turnserver.conf.sample` and ports.

## Notes

- Open UDP 8890 (SRT) and `20000-20099` (WebRTC ICE) on the host firewall.
- For scale (>100 viewers), front `media.itagenten.no/hls` with a CDN (e.g., Cloudflare).

## Best practices / hardening

- Keep compose images updated (Coolify can auto-redeploy on new tags).
- Restrict who can access admin iframes (WordPress role/capabilities).
- Consider enabling per-path auth in `mediamtx.yml` if you later need stream keys.
- Monitor MediaMTX logs in Coolify; watch host bandwidth/CPU.
