# Contributing to HomeStack Configs

## Adding a New App

1. Create `apps/<app-id>.json` following the schema in [schema.md](schema.md)
2. Add the app ID to the `apps` array in `index.json`
3. Open a PR with:
   - A working `docker run` command proving the config
   - The Docker Hub or GHCR image URL
   - The version you tested against

## Updating an Existing App

- **Version bumps** (only changing the `image` tag): Auto-approved.
- **Config changes** (ports, volumes, env): Include testing proof in the PR description.

## Rules

- `id` must be lowercase with hyphens only, and must match the filename
- Always include `"homestack"` in `networks`
- Use LinuxServer.io images when available (they follow a consistent PUID/PGID pattern)
- Set `host_subpath` explicitly and never derive host paths from container paths
- Test on Ubuntu 22.04 LTS or Debian 12 before submitting
