# list_blueprints

List and search Blueprint assets in the project via the Asset Registry.

## Input

```json
{
  "path": "/Game/Blueprints",
  "search": "Player",
  "type": "Normal",
  "include_engine": false
}
```

| Parameter        | Required | Description                                         |
| ---------------- | -------- | --------------------------------------------------- |
| `path`           | no       | Restrict to this path (default: `/Game`)            |
| `search`         | no       | Filter by name substring (case-insensitive)         |
| `type`           | no       | Filter by blueprint type (e.g. `Normal`, `Interface`) |
| `include_engine` | no       | Include engine/plugin content (default: `false`)    |

## Output

```json
{
  "count": 3,
  "blueprints": [
    {
      "name": "BP_Player",
      "asset_path": "/Game/Blueprints/BP_Player.BP_Player",
      "package_path": "/Game/Blueprints",
      "blueprint_type": "Normal",
      "parent_class": "Character",
      "parent_class_path": "/Script/CoreUObject.Class'/Script/Engine.Character'",
      "asset_class": "Blueprint"
    }
  ],
  "path_filter": "/Game/Blueprints",
  "search_filter": "Player"
}
```
