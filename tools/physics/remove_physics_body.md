# remove_physics_body

Remove a physics body from a Physics Asset by bone name or index. Also removes any constraints referencing the body.

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `asset_path` | string | Yes | Asset path of the Physics Asset. |
| `bone_name` | string | No | Bone name of the body to remove. |
| `body_index` | integer | No | Body index to remove (alternative to bone_name). |

> **Note:** At least one of `bone_name` or `body_index` must be provided.

## Response

Returns a JSON object with:

| Field | Type | Description |
|-------|------|-------------|
| `asset_path` | string | Full asset path |
| `removed_bone` | string | Bone name of the removed body |
| `removed_index` | number | Index of the removed body |
| `constraints_removed` | number | Number of constraints that were also removed |
| `body_count` | number | Total body count after removal |

## Example

```json
{
  "method": "tools/call",
  "params": {
    "name": "remove_physics_body",
    "arguments": {
      "asset_path": "/Game/PA_Character",
      "bone_name": "Arm_L"
    }
  }
}
```

Response:

```json
{
  "asset_path": "/Game/PA_Character.PA_Character",
  "removed_bone": "Arm_L",
  "removed_index": 3,
  "constraints_removed": 1,
  "body_count": 5
}
```

## Errors

| Condition | Error |
|-----------|-------|
| Missing asset_path | "asset_path is required" |
| Missing both identifiers | "Either 'bone_name' or 'body_index' is required" |
| Body not found | "Body not found: {bone_name or index}" |
| Asset not found | "Physics Asset not found: {path}" |
