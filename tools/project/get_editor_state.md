# get_editor_state

Get the current editor state: PIE/simulation status, loaded level, selected actors, active editor mode, viewport camera info, and dirty (unsaved) packages.

## Input

```json
{}
```

| Parameter                | Required | Default | Description                                        |
| ------------------------ | -------- | ------- | -------------------------------------------------- |
| `include_viewport`       | no       | `true`  | Include viewport camera info for all viewports.    |
| `include_dirty_packages` | no       | `true`  | Include list of dirty (unsaved) packages.          |

## Output

| Field                  | Type    | Description                                            |
| ---------------------- | ------- | ------------------------------------------------------ |
| `pie`                  | object  | PIE status (see below)                                 |
| `level`               | object  | Loaded level info (see below)                          |
| `num_selected_actors` | number  | Count of selected actors                               |
| `selected_actors`     | array   | Selected actors with name, label, class, location, rotation |
| `active_editor_modes` | array   | List of active editor mode names                       |
| `viewports`           | array   | Viewport camera info (see below)                       |
| `num_viewports`       | number  | Count of viewports                                     |
| `num_dirty_packages`  | number  | Count of dirty (unsaved) packages                      |
| `dirty_packages`      | array   | Package names with unsaved changes                     |

### PIE object

| Field        | Type    | Description                                |
| ------------ | ------- | ------------------------------------------ |
| `active`     | boolean | Whether Play In Editor is currently active |
| `simulating` | boolean | Whether Simulate In Editor is active       |

### Level object

| Field              | Type   | Description                             |
| ------------------ | ------ | --------------------------------------- |
| `name`             | string | Map name                                |
| `package`          | string | Package name                            |
| `path`             | string | Full path                               |
| `streaming_levels` | array  | Streaming sub-levels (name, loaded, visible) |

### Selected actor object

| Field      | Type   | Description                     |
| ---------- | ------ | ------------------------------- |
| `name`     | string | Actor name                      |
| `label`    | string | Actor label (display name)      |
| `class`    | string | Actor class name                |
| `location` | object | `{x, y, z}` world location     |
| `rotation` | object | `{pitch, yaw, roll}` rotation  |

### Viewport object

| Field      | Type    | Description                                     |
| ---------- | ------- | ----------------------------------------------- |
| `index`    | number  | Viewport index                                  |
| `location` | object  | `{x, y, z}` camera location                    |
| `rotation` | object  | `{pitch, yaw, roll}` camera rotation            |
| `fov`      | number  | Field of view angle                             |
| `realtime` | boolean | Whether viewport updates in realtime            |
| `type`     | string  | Viewport type (Perspective, OrthoXY, etc.)      |

## Examples

### Get full editor state

```json
{}
```

### Get state without viewport or dirty package info

```json
{
  "include_viewport": false,
  "include_dirty_packages": false
}
```

## Errors

This tool does not fail — it always returns available editor state information.
