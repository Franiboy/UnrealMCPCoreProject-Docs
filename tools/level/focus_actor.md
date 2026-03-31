# focus_actor

Focus the editor viewport camera on a specific actor. Frames the camera to show the actor's full bounding box. Optionally selects the actor and adjusts padding around the bounds.

## Input

```json
{
  "actor_name": "MyLight"
}
```

| Parameter        | Required | Default | Description                                                              |
| ---------------- | -------- | ------- | ------------------------------------------------------------------------ |
| `actor_name`     | **yes**  | —       | Actor name or editor label (case-insensitive exact match).               |
| `select`         | no       | `true`  | If true, also select the actor in the editor (clears previous selection).|
| `padding`        | no       | `1.5`   | Multiplier for padding around bounds (0.1–10.0). Higher = zoom out more. |
| `viewport_index` | no       | `0`     | Which viewport to focus (0 = first/active viewport).                     |

## Output

| Field              | Type    | Description                                            |
| ------------------ | ------- | ------------------------------------------------------ |
| `name`             | string  | Internal actor name                                    |
| `label`            | string  | Editor display label                                   |
| `class`            | string  | Class name                                             |
| `selected`         | boolean | Whether the actor was selected                         |
| `viewport_updated` | boolean | Whether a viewport was found and updated               |
| `bounds`           | object  | Actor bounding box (see below)                         |
| `camera`           | object  | New camera position (see below)                        |

### Bounds object

| Field      | Type   | Description                        |
| ---------- | ------ | ---------------------------------- |
| `center_x` | number | Bounding box center X              |
| `center_y` | number | Bounding box center Y              |
| `center_z` | number | Bounding box center Z              |
| `extent_x` | number | Half-extent X                      |
| `extent_y` | number | Half-extent Y                      |
| `extent_z` | number | Half-extent Z                      |
| `radius`   | number | Bounding sphere radius             |

### Camera object

| Field            | Type   | Description                    |
| ---------------- | ------ | ------------------------------ |
| `location_x`    | number | Camera world X                  |
| `location_y`    | number | Camera world Y                  |
| `location_z`    | number | Camera world Z                  |
| `rotation_pitch` | number | Camera pitch (degrees)          |
| `rotation_yaw`   | number | Camera yaw (degrees)            |
| `rotation_roll`  | number | Camera roll (degrees)           |
| `distance`       | number | Distance from camera to actor   |

## Examples

### Focus on an actor (default)

```json
{
  "actor_name": "MainCamera"
}
```

### Focus without selecting

```json
{
  "actor_name": "Floor",
  "select": false
}
```

### Focus zoomed out

```json
{
  "actor_name": "BP_Enemy_1",
  "padding": 3.0
}
```

### Focus on a specific viewport

```json
{
  "actor_name": "MyLight",
  "viewport_index": 1
}
```

## Errors

| Condition                  | Error message                                           |
| -------------------------- | ------------------------------------------------------- |
| Missing `actor_name`       | `Missing required parameter: actor_name`                |
| Editor not available       | `Editor not available`                                  |
| No editor world            | `No editor world available`                             |
| Actor not found            | `Actor '<name>' not found in the current level`         |

In headless mode (no viewport), the tool still succeeds and returns `viewport_updated: false` with the computed camera position.
