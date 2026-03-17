<!-- markdownlint-disable -->
# HomeStack App Config Schema (v2.0.0)

Every app JSON file must conform to this schema exactly.

## Top-Level Fields

| Field                  | Type                  | Required | Description                                                                              |
| ---------------------- | --------------------- | -------- | ---------------------------------------------------------------------------------------- |
| `id`                   | `string`              | ✅        | Unique app identifier. Lowercase, hyphens allowed. Must match filename.                  |
| `name`                 | `string`              | ✅        | Human-readable display name.                                                             |
| `description`          | `string`              | ✅        | One-sentence description shown in the UI.                                                |
| `category`             | `string`              | ✅        | One of: `media`, `network`, `monitoring`, `dashboard`, `productivity`, `automation`, `infrastructure`. |
| `icon_url`             | `string`              | ✅        | URL to app icon (use dashboard-icons CDN).                                               |
| `image`                | `string`              | ✅        | Docker image reference (prefer LinuxServer.io images).                                   |
| `ports`                | `Port[]`              | ✅        | Array of port mappings. Empty array for host-network or VPN services.                    |
| `volumes`              | `Volume[]`            | ✅        | Array of volume mounts.                                                                  |
| `environment`          | `EnvironmentConfig`   | ✅        | Environment variable configuration.                                                      |
| `networks`             | `string[]`            | ✅        | Docker networks (include `"homestack"`). Empty array for host-network services.           |
| `depends_on`           | `string[]`            | ✅        | App IDs this app requires. Empty array if none.                                          |
| `conflicts_with`       | `string[]`            | ✅        | App IDs that cannot coexist. Empty array if none.                                        |
| `preflight_checks`     | `PreflightCheck[]`    | ✅        | Checks to run before install. Empty array if none.                                       |
| `homepage_integration` | `HomepageIntegration` | ✅        | Homepage dashboard config.                                                               |
| `tags`                 | `string[]`            | ✅        | Searchable tags.                                                                         |
| `web_port`             | `number`              | ❌        | Explicit web UI port, overrides href parsing when present.                                |
| `network_mode`         | `string`              | ❌        | Docker network mode (e.g. `"host"`). Omit for default bridge mode.                       |
| `devices`              | `string[]`            | ❌        | Host device passthrough (e.g. `["/dev/dri:/dev/dri"]`).                                  |
| `cap_add`              | `string[]`            | ❌        | Linux capabilities (e.g. `["NET_ADMIN", "NET_RAW"]`).                                    |
| `privileged`           | `boolean`             | ❌        | Run container in privileged mode.                                                        |
| `sidecars`             | `SidecarService[]`    | ❌        | Additional containers this app requires (databases, caches, ML workers).                 |
| `setup_wizard`         | `SetupWizard`         | ❌        | Interactive post-deploy configuration steps.

## Port

| Field            | Type             | Description                           |
| ---------------- | ---------------- | ------------------------------------- |
| `host`           | `number`         | Host port number.                     |
| `container`      | `number`         | Container port number.                |
| `protocol`       | `"tcp" \| "udp"` | Port protocol.                        |
| `label`          | `string`         | Human-readable label (e.g. "Web UI"). |
| `conflicts_with` | `string[]`       | App IDs that use the same host port.  |

## Volume

| Field            | Type     | Description                                                                    |
| ---------------- | -------- | ------------------------------------------------------------------------------ |
| `host_path_key`  | `string` | Environment variable key for the base path (e.g. `"CONFIG_DIR"`).              |
| `host_subpath`   | `string` | Relative path appended to `host_path_key`. Empty string for root of base path. |
| `container_path` | `string` | Mount path inside the container.                                               |
| `label`          | `string` | Human-readable description.                                                    |

**Volume path generation:** `${host_path_key}/${host_subpath}:${container_path}` (omit `/${host_subpath}` when empty).

## EnvironmentConfig

| Field                  | Type         | Description                                   |
| ---------------------- | ------------ | --------------------------------------------- |
| `standard_linuxserver` | `boolean`    | If `true`, auto-injects `PUID`, `PGID`, `TZ`. |
| `extra_vars`           | `ExtraVar[]` | Additional environment variables.             |

## ExtraVar

| Field             | Type      | Description                                                     |
| ----------------- | --------- | --------------------------------------------------------------- |
| `key`             | `string`  | Variable name inside the container (e.g. `"WEBPASSWORD"`).      |
| `env_key`         | `string`  | Key in the generated `.env` file (e.g. `"PIHOLE_WEBPASSWORD"`). |
| `generate_secret` | `boolean` | If `true`, auto-generate a secure random value.                 |
| `description`     | `string`  | Human-readable description.                                     |

## PreflightCheck

| Field         | Type     | Description                                          |
| ------------- | -------- | ---------------------------------------------------- |
| `id`          | `string` | Unique check identifier (e.g. `"systemd_resolved"`). |
| `description` | `string` | What this check does.                                |

## HomepageIntegration

| Field         | Type              | Description                       |
| ------------- | ----------------- | --------------------------------- |
| `href`        | `string`          | URL shown in Homepage dashboard.  |
| `description` | `string`          | Service description for Homepage. |
| `icon`        | `string`          | Icon filename for Homepage.       |
| `widget`      | `HomepageWidget?` | Optional widget config.           |

## HomepageWidget

| Field  | Type      | Description                                       |
| ------ | --------- | ------------------------------------------------- |
| `type` | `string`  | Homepage widget type.                             |
| `url`  | `string`  | Internal URL for the widget to reach the service. |
| `key`  | `string?` | `.env` key for the API key (optional).            |

## SidecarService (v2.0)

Multi-container apps (e.g. Immich, Nextcloud, Paperless-ngx) define additional containers as sidecars.

| Field          | Type                    | Description                                                          |
| -------------- | ----------------------- | -------------------------------------------------------------------- |
| `id`           | `string`                | Container name / service ID.                                         |
| `image`        | `string`                | Docker image reference.                                              |
| `volumes`      | `Volume[]`              | Volume mounts (same schema as top-level).                            |
| `environment`  | `Record<string, string>`| Simple key=value env vars. No dynamic secret generation.             |
| `ports`        | `Port[]`                | Port mappings. Typically empty (internal only).                      |
| `networks`     | `string[]`              | Docker networks. Must overlap with parent for inter-container comms. |
| `depends_on`   | `string[]`              | Service IDs this sidecar depends on.                                 |
| `healthcheck`  | `DockerHealthcheck?`    | Optional healthcheck configuration.                                  |

## DockerHealthcheck (v2.0)

| Field      | Type       | Description                                               |
| ---------- | ---------- | --------------------------------------------------------- |
| `test`     | `string[]` | Healthcheck command (e.g. `["CMD-SHELL", "pg_isready"]`). |
| `interval` | `string`   | Check interval (e.g. `"30s"`).                            |
| `timeout`  | `string`   | Timeout per check (e.g. `"5s"`).                          |
| `retries`  | `number`   | Retry count before marking unhealthy.                     |

## SetupWizard (v2.0)

Interactive post-deploy configuration steps shown in the TUI installer.

| Field          | Type                | Description                                                |
| -------------- | ------------------- | ---------------------------------------------------------- |
| `steps`        | `SetupWizardStep[]` | Ordered configuration steps.                               |
| `web_url_path` | `string \| null`    | Path appended to service URL to open in browser. Null if external. |
| `docs_url`     | `string?`           | Link to the app's documentation.                           |

## SetupWizardStep (v2.0)

| Field         | Type                                       | Description                                     |
| ------------- | ------------------------------------------ | ----------------------------------------------- |
| `instruction` | `string`                                   | Human-readable step instruction.                |
| `type`        | `"info" \| "input" \| "get_key"`           | Step type. `info` = display only, `input` = user must provide a value, `get_key` = open URL to get a key. |
| `env_key`     | `string?`                                  | For `input`/`get_key`: which `.env` key to fill. |
| `url`         | `string?`                                  | For `get_key`: URL where user obtains the key.  |
