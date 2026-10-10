This app installs a Minecraft Java Edition dedicated server using the
Fabric mod loader, with the server data (worlds, mods, configs, web map
tiles) stored in the standard YunoHost app data directory — fully managed
by `yunohost backup` and outside any Nextcloud data directory.

Features:

- Fabric mod loader, any Minecraft Java version the Fabric installer knows
- Pinned Temurin Java runtime (no system Java dependency)
- Web dashboard URL for the Voxeldash mod (when installed)
- Optional dedicated domain for the BlueMap live web map (when installed),
  served publicly so players without a YunoHost account can view the map
- Managed game port (TCP+UDP) and Simple Voice Chat port (UDP), opened in
  the firewall automatically
- Import of a pre-existing server directory at install time (world, mods,
  configs, map data are kept; binaries are freshly provisioned)
- Full backup/restore support of all game data

Requirements:

- ~4 GB free disk for a typical small server (world + map tiles)
- 3+ GB free RAM for the JVM on a lightly-modded server
- Behind NAT you must still port-forward TCP+UDP 25565 (and UDP 24454 for
  voice chat) on your router
