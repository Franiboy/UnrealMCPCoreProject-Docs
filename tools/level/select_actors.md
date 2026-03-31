# select_actors

Select actors in the editor viewport by name, class, tag, folder, or search filter. Provides visual feedback in the viewport — selected actors are highlighted. Supports replace, add, and remove selection modes.

## Input

```json
{
  "class": "PointLight"
}
```

| Parameter      | Required | Default     | Description                                                                    |
| -------------- | -------- | ----------- | ------------------------------------------------------------------------------ |
| `actor_names`  | no       | —           | Array of actor names to select. Matches internal name or editor label (case-insensitive exact match). |
| `class`        | no       | —           | Select actors of this class (case-insensitive partial match, e.g. `PointLight`). |
| `tag`          | no       | —           | Select actors with this tag (case-insensitive exact match).                    |
| `search`       | no       | —           | Select actors whose name or label contains this substring (case-insensitive).  |
| `folder`       | no       | —           | Select actors in this editor folder path (prefix match).                       |
| `mode`         | no       | `"replace"` | Selection mode: `replace` clears first, `add` adds to current, `remove` deselects matching. |

At least one filter (`actor_names`, `class`, `tag`, `search`, or `folder`) is required. Multiple filters are combined with AND logic.

## Output

| Field             | Type   | Description                                |
| ----------------- | ------ | ------------------------------------------ |
| `mode`            | string | Selection mode used                        |
| `matched_count`   | number | Number of actors matching the filters      |
| `total_selected`  | number | Total selected actors after the operation  |
| `matched_actors`  | array  | Array of matched actor objects (see below) |

### Matched actor object

| Field   | Type   | Description                              |
| ------- | ------ | ---------------------------------------- |
| `name`  | string | Internal actor name                      |
| `label` | string | Editor display label                     |
| `class` | string | Class name (e.g. `PointLight`)           |

## Examples

### Select all PointLights

```json
{
  "class": "PointLight"
}
```

### Select specific actors by name

```json
{
  "actor_names": ["MyLight", "MainCamera"]
}
```

### Add to current selection by tag

```json
{
  "tag": "Interactive",
  "mode": "add"
}
```

### Remove actors from selection by class

```json
{
  "class": "StaticMeshActor",
  "mode": "remove"
}
```

### Select by search substring

```json
{
  "search": "Enemy"
}
```

### Combined filters (AND logic)

```json
{
  "class": "PointLight",
  "folder": "Lighting"
}
```

## Errors

| Condition                  | Error message                                                         |
| -------------------------- | --------------------------------------------------------------------- |
| Editor not available       | `Editor not available`                                                |
| No editor world            | `No editor world available`                                           |
| No filters provided        | `At least one filter is required: actor_names, class, tag, search, or folder` |
| Invalid mode               | `Invalid mode '<mode>'. Use 'replace', 'add', or 'remove'.`          |

Returns `matched_count: 0` if no actors match the filters (not an error).
