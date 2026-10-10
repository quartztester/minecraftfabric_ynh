# Admin notes

## First steps after install

1. Forward **TCP+UDP __PORT__** (game) and **UDP __PORT_VOICE__** (voice
   chat, if the mod is installed) on your router to this server — behind
   NAT the firewall being open is not enough.
2. Point your Minecraft client at `your-domain-or-IP:__PORT__`.
3. Ops on the data itself (versions, mods, whitelist) happen in
   `__DATA_DIR__`, not in the admin UI.

## Ports

| purpose | port | notes |
|---------|------|-------|
| game (TCP play, UDP query) | `__PORT__` | fixed; router-forward TCP+UDP |
| Simple Voice Chat (UDP) | `__PORT_VOICE__` | only used when the mod jar is in `mods/` |
| BlueMap web server | `__PORT_WEB__` | loopback only; exposed via nginx on `bluemap_domain` |
| Voxeldash dashboard | `__PORT_DASH__` | loopback only; exposed via nginx on the app domain |

## Routine settings

| want | how |
|------|-----|
| change heap | `yunohost app setting __APP__ ram_mb -v 4096 && systemctl restart __APP__` |
| restart server | `systemctl restart __APP__` (players get ~30 s downtime; world saves safely) |
| live console | `htop`-style: watch `journalctl -u __APP__ -f` — no RCON is enabled |
| game settings (motd, whitelist, max-players) | edit `__DATA_DIR__/server.properties`, restart |
| add a mod | drop jar into `__DATA_DIR__/mods/`, restart |
| upgrade MC version | `yunohost app upgrade __APP__ -u <dir>` after bumping `minecraft_version` — only on a **backed-up** world; major version jumps mutate the world irreversibly |

## Data & identity

Everything is in `__DATA_DIR__`: worlds (`world*`), `mods/`, `config/`
(Voxeldash DB lives in `config/voxeldash/`), `bluemap/` (map tiles +
config), logs. `yunohost backup create --apps __APP__` archives it all;
restore puts it back and re-renders nginx. **Do not** sync this dir with a
Nextcloud/ownCloud client — that is the whole reason this package exists.

App upgrades re-provision the JRE/launcher only; your data and mods are
never rewritten.

## Troubleshooting

| symptom | cause & fix |
|---------|-------------|
| server runs but nobody connects from outside | missing router port-forward for TCP+UDP `__PORT__` (NAT). Works fine from LAN |
| `Address already in use` at start | an old hand-written unit/container holds 25565: `ss -tlnp \| grep 25565`, stop the other thing |
| dashboard 502 at `https://__DOMAIN__/` | Voxeldash mod not installed or still booting — server answers before mods' web servers bind; wait for `Done (` in `journalctl -u __APP__` |
| BlueMap domain 502 | BlueMap mod absent/disabled, or `bluemap_domain` setting wrong: `yunohost app setting __APP__ bluemap_domain` |
| BlueMap map stale | it renders in the background — check `__DATA_DIR__/bluemap/logs/`; heavy first renders take hours |
| world won't load after MC version bump | downgrade `minecraft_version` back and restore a backup — never run newer-version worlds on older jars |
| voice chat works on LAN not WAN | UDP `__PORT_VOICE__` not forwarded; voice needs UDP end-to-end |
