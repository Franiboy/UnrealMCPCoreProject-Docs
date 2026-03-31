# create_material_instance

Create a new `MaterialInstanceConstant` from a parent material or material instance, with optional scalar, vector, and texture parameter overrides.

## Input

```json
{
  "name": "MI_Floor_Red",
  "parent": "/Game/Materials/M_Floor",
  "path": "/Game/Materials",
  "scalar_parameters": [
    {"name": "Roughness", "value": 0.3}
  ],
  "vector_parameters": [
    {"name": "BaseColor", "value": {"r": 1.0, "g": 0.0, "b": 0.0, "a": 1.0}}
  ],
  "texture_parameters": [
    {"name": "DiffuseTexture", "value": "/Game/Textures/T_BrickRed"}
  ]
}
```

| Parameter            | Required | Description                                                                      |
| -------------------- | -------- | -------------------------------------------------------------------------------- |
| `name`               | **yes**  | Instance asset name (e.g. `MI_Floor_Red`)                                        |
| `parent`             | **yes**  | Content path to the parent material or material instance                         |
| `path`               | no       | Content folder path (default: `/Game`)                                           |
| `scalar_parameters`  | no       | Array of scalar parameter overrides: `[{"name": "...", "value": 0.5}]`           |
| `vector_parameters`  | no       | Array of vector parameter overrides: `[{"name": "...", "value": {"r","g","b","a"}}]` |
| `texture_parameters` | no       | Array of texture parameter overrides: `[{"name": "...", "value": "/Game/..."}]`  |

### Parameter Override Format

**Scalar:**
```json
{"name": "Roughness", "value": 0.75}
```

**Vector (linear color):**
```json
{"name": "BaseColor", "value": {"r": 1.0, "g": 0.5, "b": 0.0, "a": 1.0}}
```

**Texture:**
```json
{"name": "DiffuseTexture", "value": "/Game/Textures/T_MyTexture"}
```

## Output

```json
{
  "name": "MI_Floor_Red",
  "asset_path": "/Game/Materials/MI_Floor_Red",
  "class": "MaterialInstanceConstant",
  "parent": "/Game/Materials/M_Floor.M_Floor",
  "base_material": "/Game/Materials/M_Floor.M_Floor",
  "parameter_overrides": {
    "scalar": 1,
    "vector": 1,
    "texture": 1
  }
}
```

| Field                          | Type   | Description                                               |
| ------------------------------ | ------ | --------------------------------------------------------- |
| `name`                         | string | Created instance asset name                               |
| `asset_path`                   | string | Full content path to the new instance                     |
| `class`                        | string | Always `MaterialInstanceConstant`                         |
| `parent`                       | string | Full path to the parent material                          |
| `base_material`                | string | Ultimate base material (resolved through parent chain)    |
| `parameter_overrides.scalar`   | number | Count of scalar parameters overridden at creation         |
| `parameter_overrides.vector`   | number | Count of vector parameters overridden at creation         |
| `parameter_overrides.texture`  | number | Count of texture parameters overridden at creation        |

## Examples

### Basic instance (no overrides)

```json
{
  "name": "MI_BasicFloor",
  "parent": "/Game/Materials/M_Floor"
}
```

### Instance with color and roughness

```json
{
  "name": "MI_Floor_Shiny",
  "parent": "/Game/Materials/M_Floor",
  "scalar_parameters": [{"name": "Roughness", "value": 0.1}],
  "vector_parameters": [{"name": "BaseColor", "value": {"r": 0.8, "g": 0.8, "b": 0.9, "a": 1.0}}]
}
```

### Chain instances (instance from instance)

```json
{
  "name": "MI_Floor_ShinyRed",
  "parent": "/Game/Materials/MI_Floor_Shiny"
}
```

## Notes

- The parent can be a base `UMaterial` or another `MaterialInstanceConstant` (chaining)
- Texture parameter values must be valid content paths to loaded texture assets; unresolvable textures are skipped with a warning
- Parameter names do not need to match existing parent parameters — they are stored as overrides regardless
- Use `get_material_info` on the created instance to inspect the full parameter state

## Errors

| Condition                | `isError` | Message                                          |
| ------------------------ | --------- | ------------------------------------------------ |
| Missing `name`           | `true`    | `Missing required parameter 'name'`              |
| Missing `parent`         | `true`    | `Missing required parameter 'parent'`            |
| Parent not found         | `true`    | `Parent material not found: '/Game/...'`         |
| Asset already exists     | `true`    | `An asset already exists at '/Game/...'`         |
| Invalid `path`           | `true`    | `Invalid path '...'. Must start with /Game or /Engine.` |
