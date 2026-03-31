# create_physics_asset

Create a new empty Physics Asset.

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `name` | string | Yes | Name of the Physics Asset. |
| `path` | string | No | Content path (default: `/Game`). |

## Response

Returns a JSON object with:

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Created asset name |
| `asset_path` | string | Full asset path |
| `body_count` | number | Number of bodies (0 for new asset) |
| `constraint_count` | number | Number of constraints (0 for new asset) |

## Example

```json
{
  "method": "tools/call",
  "params": {
    "name": "create_physics_asset",
    "arguments": {
      "name": "PA_Ragdoll",
      "path": "/Game/Physics"
    }
  }
}
```

Response:

```json
{
  "name": "PA_Ragdoll",
  "asset_path": "/Game/Physics/PA_Ragdoll.PA_Ragdoll",
  "body_count": 0,
  "constraint_count": 0
}
```

## Errors

| Condition | Error |
|-----------|-------|
| Missing name | "'name' is required" |
| Asset already exists | "Physics Asset already exists: {path}" |
| Creation failed | "Failed to create Physics Asset: {path}" |
