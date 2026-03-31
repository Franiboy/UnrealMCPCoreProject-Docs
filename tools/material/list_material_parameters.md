# list_material_parameters

List all parameters of a material or material instance, organized by type (scalar, vector, texture, static_switch). Returns parameter names, current values, groups, and override status.

## Input

```json
{
  "asset_path": "/Game/Materials/M_Basic_Floor"
}
```

| Parameter    | Required | Description                                                       |
| ------------ | -------- | ----------------------------------------------------------------- |
| `asset_path` | **yes**  | Content path to the material or material instance                 |

## Output

```json
{
  "name": "M_Basic_Floor",
  "asset_path": "/Game/StarterContent/Materials/M_Basic_Floor.M_Basic_Floor",
  "class": "Material",
  "scalar": [
    {"name": "Roughness", "value": 0.5, "group": "Surface"}
  ],
  "vector": [
    {"name": "BaseColor", "value": {"r": 1.0, "g": 1.0, "b": 1.0, "a": 1.0}}
  ],
  "texture": [
    {"name": "DiffuseTexture", "value": "/Game/Textures/T_Brick.T_Brick"}
  ],
  "static_switch": [],
  "total_parameters": 3
}
```

| Field               | Type   | Description                                         |
| ------------------- | ------ | --------------------------------------------------- |
| `name`              | string | Asset name                                          |
| `asset_path`        | string | Full content path                                   |
| `class`             | string | Asset class (`Material`, `MaterialInstanceConstant`) |
| `scalar`            | array  | Scalar parameters with name, value, group, overridden |
| `vector`            | array  | Vector parameters with name, value (r,g,b,a), group  |
| `texture`           | array  | Texture parameters with name, value (path), group     |
| `static_switch`     | array  | Static switch parameters with name, value (bool)      |
| `total_parameters`  | number | Total count across all parameter types                |

### Parameter Object Fields

Each parameter object contains:

| Field        | Type    | Description                              |
| ------------ | ------- | ---------------------------------------- |
| `name`       | string  | Parameter name                           |
| `value`      | varies  | Current value (number, color object, path, or bool) |
| `group`      | string  | Parameter group (optional, if assigned)  |
| `overridden` | boolean | Whether this parameter is overridden (instances only) |

## Examples

### List parameters of a base material

```json
{
  "asset_path": "/Game/StarterContent/Materials/M_Basic_Floor"
}
```

### List parameters of a material instance

```json
{
  "asset_path": "/Game/Materials/MI_Floor_Red"
}
```

## Notes

- Lighter-weight alternative to `get_material_info` when you only need parameter data (no domain, blend mode, expressions, etc.)
- Works with both base materials (`UMaterial`) and material instances (`MaterialInstanceConstant`)
- `static_switch` parameters are only available in editor builds (`WITH_EDITORONLY_DATA`)

## Errors

| Condition          | `isError` | Message                              |
| ------------------ | --------- | ------------------------------------ |
| Missing `asset_path` | `true`  | `Missing required parameter 'asset_path'` |
| Asset not found    | `true`    | `Material not found: '...'`          |
