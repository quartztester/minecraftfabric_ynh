# Managing mods

Mods live in `__DATA_DIR__/mods/` and are managed by the admin, not the
package: drop or remove jars, then `systemctl restart __APP__`. An app
upgrade never touches `mods/`, `world/`, or `config/`. Fabric mods must
match the Minecraft version — for version changes, check each mod's
Modrinth page first.

Two mods have **nginx fronting provided by this package** and are wired to
its port settings automatically on import:

- `voxeldash.jar` — dashboard on the app's domain
- `bluemap.jar` — live map on `bluemap_domain` (config auto-realigned to
  `__PORT_WEB__` at import; later changes in `config/bluemap/webserver.conf`
  must keep that port and the `127.0.0.1` bind)

## Example mod set (Wildest Dreams server, MC 26.2, Fabric)

The reference set this package was developed and live-tested against:

| mod | purpose |
|-----|---------|
| fabric-api, fabric-language-kotlin | loader prerequisites for other mods |
| bluemap, bluemap-offline-player-markers | live 3D web map + offline player markers |
| voxeldash | web dashboard (players/console/files/worlds) |
| voicechat (Simple Voice Chat) | proximity voice, UDP `__PORT_VOICE__` |
| serverreplay | server-side replay recording |
| c2me, lithium, krypton, ferritecore, servercore, slo, scalablelux | performance/memory/network optimizations |
| chunky | pre-generates chunks (do this before inviting players!) |
| ksyxis, sparsestructures | faster world loading / lighter + tuned structure gen |
| death-count | tracks player death counts |
| appleskin | food-value tooltips (client-side, harmless on server) |
| resourcefulconfig | config library required by some mods |

Non-default server settings worth copying: `max-players=10`, whitelist
empty (open server, online-mode auth), C2ME/`chunky` combo for pregen.
