# After installation

The Minecraft server is installed and running. This page summarizes what
to check first.

## Join the game

Add the server in Minecraft with address **`__DOMAIN__`** (port `__PORT__`
is used automatically if the domain resolves to this server; otherwise use
`your-ip:__PORT__`).

## Required next step: open the game to the Internet

If this server is behind a router/NAT, forward:

| Protocol | Port | Purpose |
|----------|------|---------|
| TCP + UDP | `__PORT__` | the game itself (UDP = latency pings) |
| UDP | `__PORT_VOICE__` | Simple Voice Chat mod (only if installed) |

If this domain's DNS goes through a proxy/CDN (e.g. Cloudflare orange
cloud), the record must be DNS-only — proxied records drop raw game traffic.

## Web UIs

- **Dashboard:** https://`__DOMAIN__`/ (proxies the Voxeldash mod; players
  without YunoHost accounts need this to be public)
- **BlueMap map:** `bluemap_domain` if configured — public by design, tiles
  render gradually after first start

## Do not

- **Do not sync the data directory with Nextcloud/sync clients** — the whole
  point of this package is keeping world churn out of file scanners. Data
  lives in `__DATA_DIR__` and is archived by `yunohost backup`.
- Update Minecraft versions only after a backup (`yunohost backup create
  --apps __APP__`); major version jumps mutate the world irreversibly.

Further administration, ports table, and troubleshooting: see the app's
**Documentation** tab (admin doc) in the YunoHost admin.
