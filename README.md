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

## Current Apps (v1.0)

| App         | Category   | Description                    |
| ----------- | ---------- | ------------------------------ |
| Jellyfin    | Media      | Free open-source media server  |
| Radarr      | Media      | Automatic movie downloading    |
| Sonarr      | Media      | Automatic TV show downloading  |
| Overseerr   | Media      | Media request management       |
| Pi-hole     | Network    | DNS-level ad blocking          |
| Uptime Kuma | Monitoring | Self-hosted service monitoring |
| Homepage    | Dashboard  | Customisable service dashboard |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to add or update apps.

## License

MIT
