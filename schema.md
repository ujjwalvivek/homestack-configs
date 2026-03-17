<!-- markdownlint-disable -->
# HomeStack App Config Schema (v1.0.0)

Every app JSON file must conform to this schema exactly.

## Top-Level Fields

| Field                  | Type                  | Required | Description                                                             |
| ---------------------- | --------------------- | -------- | ----------------------------------------------------------------------- |
| `id`                   | `string`              | ✅        | Unique app identifier. Lowercase, hyphens allowed. Must match filename. |
| `name`                 | `string`              | ✅        | Human-readable display name.                                            |
| `description`          | `string`              | ✅        | One-sentence description shown in the UI.                               |
| `category`             | `string`              | ✅        | One of: `media`, `network`, `monitoring`, `dashboard`.                  |
| `icon_url`             | `string`              | ✅        | URL to app icon (use dashboard-icons CDN).                              |
| `image`                | `string`              | ✅        | Docker image reference (prefer LinuxServer.io images).                  |
| `ports`                | `Port[]`              | ✅        | Array of port mappings.                                                 |
| `volumes`              | `Volume[]`            | ✅        | Array of volume mounts.                                                 |
| `environment`          | `EnvironmentConfig`   | ✅        | Environment variable configuration.                                     |
| `networks`             | `string[]`            | ✅        | Docker networks (always include `"homestack"`).                         |
| `depends_on`           | `string[]`            | ✅        | App IDs this app requires. Empty array if none.                         |
| `conflicts_with`       | `string[]`            | ✅        | App IDs that cannot coexist. Empty array if none.                       |
| `preflight_checks`     | `PreflightCheck[]`    | ✅        | Checks to run before install. Empty array if none.                      |
| `homepage_integration` | `HomepageIntegration` | ✅        | Homepage dashboard config.                                              |
| `tags`                 | `string[]`            | ✅        | Searchable tags.                                                        |

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
