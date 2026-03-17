<!-- markdownlint-disable -->
# HomeStack App Configurations

Community-maintained Docker app configurations for [HomeStack](https://homestack.sh).

## Structure

```
apps/           ← One JSON file per app
index.json      ← Master list of all app IDs
schema.md       ← Field-by-field schema documentation
CONTRIBUTING.md ← How to add or update apps
```

## How It Works

HomeStack reads these JSON configs at runtime to generate `docker-compose.yml`, `.env`, and `setup.sh` files. Each JSON file fully describes one Docker app, its image, ports, volumes, environment variables, preflight checks, and homepage integration.

## Current Apps (v2.0: 19 apps)

| App            | Category       | Description                                 |
| -------------- | -------------- | ------------------------------------------- |
| Jellyfin       | Media          | Free open-source media server               |
| Plex           | Media          | Stream media anywhere, hardware transcoding |
| Radarr         | Media          | Automatic movie downloading                 |
| Sonarr         | Media          | Automatic TV show downloading               |
| Overseerr      | Media          | Media request management                    |
| Prowlarr       | Media          | Indexer manager for Sonarr/Radarr           |
| Whisparr       | Media          | Adult content automation (Sonarr fork)      |
| qBittorrent    | Media          | Torrent client with web UI                  |
| Immich         | Media          | Self-hosted Google Photos alternative       |
| Nextcloud      | Productivity   | Self-hosted cloud storage & collaboration   |
| Paperless-ngx  | Productivity   | Document management with OCR                |
| Stirling PDF   | Productivity   | All-in-one PDF tools                        |
| Home Assistant | Automation     | Open-source home automation platform        |
| Pi-hole        | Network        | DNS-level ad blocking                       |
| Uptime Kuma    | Monitoring     | Self-hosted service monitoring              |
| Homepage       | Dashboard      | Customisable service dashboard              |
| Caddy          | Infrastructure | Automatic HTTPS reverse proxy               |
| Dockge         | Infrastructure | Docker Compose manager UI                   |
| Tailscale      | Infrastructure | Zero-config VPN for remote access           |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to add or update apps.

## License

MIT
