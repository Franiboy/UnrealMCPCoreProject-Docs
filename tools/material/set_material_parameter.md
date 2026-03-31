# set_material_parameter

Set a scalar, vector, or texture parameter on a `MaterialInstanceConstant`. The instance is saved to disk after modification.

## Input

```json
{
  "asset_path": "/Game/Materials/MI_Floor",
  "parameter_name": "Roughness",
  "parameter_type": "scalar",
  "value": 0.75
}
```

| Parameter        | Required | Description                                                                 |
| ---------------- | -------- | --------------------------------------------------------------------------- |
| `asset_path`     | **yes**  | Content path to the material instance                                       |
| `parameter_name` | **yes**  | Parameter name to set                                                       |
| `parameter_type` | **yes**  | Parameter type: `scalar`, `vector`, or `texture`                            |
| `value`          | **yes**  | Parameter value (format depends on type, see below)                         |

### Value Format

**Scalar** — a number:
```json
"value": 0.75
```

**Vector** — an object with `r`, `g`, `b`, `a` fields (linear color):
```json
"value": {"r": 1.0, "g": 0.0, "b": 0.0, "a": 1.0}
```

**Texture** — a content path string:
```json
"value": "/Game/Textures/T_MyTexture"
```

## Output

```json
{
  "asset_path": "/Game/Materials/MI_Floor.MI_Floor",
  "parameter_name": "Roughness",
  "parameter_type": "scalar",
  "value": 0.75
}
```

| Field            | Type   | Description                                  |
| ---------------- | ------ | -------------------------------------------- |
| `asset_path`     | string | Full content path to the material instance   |
| `parameter_name` | string | Name of the parameter that was set           |
| `parameter_type` | string | Normalized type (`scalar`, `vector`, `texture`) |
| `value`          | varies | The value that was applied                   |

## Examples

### Set roughness

```json
{
  "asset_path": "/Game/Materials/MI_Floor",
  "parameter_name": "Roughness",
  "parameter_type": "scalar",
  "value": 0.1
}
```

### Set base color

```json
{
  "asset_path": "/Game/Materials/MI_Floor",
  "parameter_name": "BaseColor",
  "parameter_type": "vector",
  "value": {"r": 0.8, "g": 0.2, "b": 0.1, "a": 1.0}
}
```

### Set diffuse texture

```json
{
  "asset_path": "/Game/Materials/MI_Floor",
  "parameter_name": "DiffuseTexture",
  "parameter_type": "texture",
  "value": "/Game/Textures/T_BrickRed"
}
```

## Notes

- Only works on `MaterialInstanceConstant` assets. Attempting to set parameters on a base `UMaterial` returns an error.
- The `parameter_type` is case-insensitive (`Scalar`, `SCALAR`, `scalar` all work).
- Parameter names do not need to match existing parent parameters — they are stored as overrides regardless.
- The asset is saved to disk after modification.

## Errors

| Condition                      | `isError` | Message                                                          |
| ------------------------------ | --------- | ---------------------------------------------------------------- |
| Missing `asset_path`           | `true`    | `Missing required parameter 'asset_path'`                        |
| Missing `parameter_name`       | `true`    | `Missing required parameter 'parameter_name'`                    |
| Missing `parameter_type`       | `true`    | `Missing required parameter 'parameter_type'`                    |
| Missing `value`                | `true`    | `Missing required parameter 'value'`                             |
| Invalid `parameter_type`       | `true`    | `Invalid parameter_type '...'. Valid: scalar, vector, texture`   |
| Asset not found                | `true`    | `Material instance not found: '...'`                             |
| Asset is not an instance       | `true`    | `Asset '...' is a Material, not a MaterialInstanceConstant...`   |
| Scalar value not a number      | `true`    | `Scalar parameter 'value' must be a number`                      |
| Vector value not an object     | `true`    | `Vector parameter 'value' must be an object with r, g, b, a...` |
| Texture not found              | `true`    | `Texture not found: '...'`                                       |
