# compile_blueprint

Compile one or more Blueprints and report status, warnings, and errors.

## Input (single)

```json
{
  "asset_path": "/Game/BP_MyActor"
}
```

## Input (batch)

```json
{
  "asset_paths": ["/Game/BP_One", "/Game/BP_Two"]
}
```

| Parameter      | Required | Description                                                                  |
| -------------- | -------- | ---------------------------------------------------------------------------- |
| `asset_path`   | no*      | Asset path of a single Blueprint to compile                                  |
| `asset_paths`  | no*      | Array of asset paths to compile multiple Blueprints at once                  |

\* Use either `asset_path` or `asset_paths`, not both.

## Output (batch)

```json
{
  "total": 2,
  "success_count": 2,
  "fail_count": 0,
  "results": [
    {
      "asset_path": "/Game/BP_One",
      "success": true,
      "name": "BP_One",
      "old_status": "Dirty",
      "new_status": "UpToDate",
      "messages": [
        {"severity": "Info", "message": "..."}
      ]
    }
  ]
}
```
