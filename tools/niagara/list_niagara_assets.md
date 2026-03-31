# list_niagara_assets

List Niagara System and Emitter assets in the project. Returns name, path, type, emitter count (for systems), and basic metadata.

## Input

```json
{
  "path_prefix": "/Game",
  "name_filter": "Fire",
  "type_filter": "System",
  "include_engine": false
}
```

| Parameter        | Required | Description                                                    |
| ---------------- | -------- | -------------------------------------------------------------- |
| `path_prefix`    | no       | Content path prefix to search (default: `/Game`)               |
| `name_filter`    | no       | Filter by name (case-insensitive substring match)              |
| `type_filter`    | no       | `System`, `Emitter`, or `All` (default: `All`)                 |
| `include_engine` | no       | Include engine/plugin assets (default: `false`)                |

## Output

```json
{
  "count": 2,
  "assets": [
    {
      "name": "NS_Fire",
      "path": "/Game/FX/NS_Fire.NS_Fire",
      "type": "System",
      "emitter_count": 3,
      "is_valid": true
    },
    {
      "name": "NS_FireSmoke",
      "path": "/Game/FX/NS_FireSmoke.NS_FireSmoke",
      "type": "System",
      "emitter_count": 2,
      "is_valid": true
    }
  ]
}
```

| Field                   | Type    | Description                                        |
| ----------------------- | ------- | -------------------------------------------------- |
| `count`                 | number  | Total number of matching assets                    |
| `assets`                | array   | Array of asset objects                              |
| `assets[].name`         | string  | Asset name                                          |
| `assets[].path`         | string  | Full asset path                                     |
| `assets[].type`         | string  | `System` or `Emitter`                               |
| `assets[].emitter_count`| number  | Number of emitters (systems only)                   |
| `assets[].is_valid`     | boolean | Whether the system is valid (systems only)          |

## Examples

### List all game Niagara assets

```json
{}
```

### List only systems with name filter

```json
{"type_filter": "System", "name_filter": "Fire"}
```

### Include engine templates

```json
{"include_engine": true}
```

## Errors

This tool does not produce errors — it always returns a (possibly empty) result set.
