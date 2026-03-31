# clear_parameter_override

Clear/reset a parameter override on a `MaterialInstanceConstant`, reverting it to the parent material's value. Supports scalar, vector, texture, and static_switch parameter types. The instance is saved to disk after modification.

## Input

```json
{
  "asset_path": "/Game/Materials/MI_MyInstance",
  "parameter_name": "Roughness",
  "parameter_type": "scalar"
}
```

| Parameter        | Required | Description                                                                       |
| ---------------- | -------- | --------------------------------------------------------------------------------- |
| `asset_path`     | **yes**  | Content path to the material instance (e.g. `/Game/Materials/MI_MyInst`)          |
| `parameter_name` | **yes**  | Name of the parameter override to clear                                           |
| `parameter_type` | **yes**  | Type: `scalar`, `vector`, `texture`, or `static_switch`                           |

## Output

```json
{
  "name": "MI_MyInstance",
  "asset_path": "/Game/Materials",
  "parameter_name": "Roughness",
  "parameter_type": "scalar",
  "cleared": true
}
```

| Field            | Type    | Description                                |
| ---------------- | ------- | ------------------------------------------ |
| `name`           | string  | Material instance asset name               |
| `asset_path`     | string  | Package path of the material instance      |
| `parameter_name` | string  | The parameter that was cleared             |
| `parameter_type` | string  | The type of the cleared parameter          |
| `cleared`        | boolean | Always `true` on success                   |

## Examples

### Clear a scalar parameter override

```json
{
  "asset_path": "/Game/Materials/MI_Ground",
  "parameter_name": "Roughness",
  "parameter_type": "scalar"
}
```

Response:

```json
{
  "name": "MI_Ground",
  "asset_path": "/Game/Materials",
  "parameter_name": "Roughness",
  "parameter_type": "scalar",
  "cleared": true
}
```

### Clear a static switch override

```json
{
  "asset_path": "/Game/Materials/MI_Ground",
  "parameter_name": "UseDetail",
  "parameter_type": "static_switch"
}
```

Response:

```json
{
  "name": "MI_Ground",
  "asset_path": "/Game/Materials",
  "parameter_name": "UseDetail",
  "parameter_type": "static_switch",
  "cleared": true
}
```

## Notes

- Only works on `UMaterialInstanceConstant` assets — base `UMaterial` assets are not supported
- Returns an error if the named parameter has no active override on the instance
- For `static_switch` parameters, the shader permutation is updated after clearing
- The material instance is saved to disk after modification
- The `parameter_type` field is case-insensitive

## Errors

| Condition                          | `isError` | Message                                                                    |
| ---------------------------------- | --------- | -------------------------------------------------------------------------- |
| Missing `asset_path`               | `true`    | `Missing required parameter 'asset_path'`                                  |
| Missing `parameter_name`           | `true`    | `Missing required parameter 'parameter_name'`                              |
| Missing `parameter_type`           | `true`    | `Missing required parameter 'parameter_type'`                              |
| Invalid `parameter_type`           | `true`    | `Invalid parameter_type '...'. Must be one of: scalar, vector, texture, static_switch` |
| No override found                  | `true`    | `No override found for parameter '...' of type '...' on '...'`            |
| Asset not found                    | `true`    | `Material instance not found: '...'`                                       |
| Asset is a base material           | `true`    | `Asset '...' is not a MaterialInstanceConstant`                            |
