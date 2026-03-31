# get_material_info

Inspect a material or material instance: domain, blend mode, shading model, flags, parameters (scalar, vector, texture, static switch), and expression graph.

## Input

```json
{
  "asset_path": "/Game/Materials/M_MyMaterial",
  "include_parameters": true,
  "include_expressions": false
}
```

| Parameter             | Required | Description                                                                    |
| --------------------- | -------- | ------------------------------------------------------------------------------ |
| `asset_path`          | **yes**  | Material path (short `/Game/Mat/M_Foo` or full `/Game/Mat/M_Foo.M_Foo`)        |
| `include_parameters`  | no       | Include scalar, vector, texture, and static switch parameter arrays (default: `true`) |
| `include_expressions` | no       | Include expression node graph for base materials (default: `false`)            |

## Output

```json
{
  "name": "M_Basic_Floor",
  "asset_path": "/Game/StarterContent/Materials/M_Basic_Floor.M_Basic_Floor",
  "class": "Material",
  "material_type": "Material",
  "base_material": "/Game/StarterContent/Materials/M_Basic_Floor.M_Basic_Floor",
  "domain": "Surface",
  "blend_mode": "Opaque",
  "shading_model": "DefaultLit",
  "two_sided": false,
  "is_masked": false,
  "is_translucent": false,
  "parameters": {
    "scalar": [
      {
        "name": "Roughness",
        "value": 0.5,
        "group": "Material",
        "id": "ABC-123..."
      }
    ],
    "vector": [
      {
        "name": "BaseColor",
        "value": { "r": 1.0, "g": 1.0, "b": 1.0, "a": 1.0 },
        "group": "Material",
        "id": "DEF-456..."
      }
    ],
    "texture": [
      {
        "name": "DiffuseTexture",
        "value": "/Game/Textures/T_Floor.T_Floor",
        "texture_class": "Texture2D",
        "id": "GHI-789..."
      }
    ],
    "static_switch": []
  }
}
```

### Material Instance Output

For `MaterialInstanceConstant` or `MaterialInstanceDynamic` assets, the output additionally includes:

```json
{
  "material_type": "MaterialInstanceConstant",
  "parent": "/Game/Materials/M_Parent.M_Parent",
  "parameters": {
    "scalar": [
      {
        "name": "Roughness",
        "value": 0.8,
        "overridden": true
      }
    ]
  }
}
```

### Expressions Output (base materials only)

When `include_expressions` is `true` and the asset is a base `UMaterial`:

```json
{
  "expression_count": 12,
  "expressions": [
    {
      "class": "MaterialExpressionTextureSample",
      "caption": "Texture Sample",
      "description": "",
      "editor_x": -320,
      "editor_y": 128,
      "id": "JKL-012..."
    }
  ]
}
```

## Field Reference

| Field              | Type    | Description                                                              |
| ------------------ | ------- | ------------------------------------------------------------------------ |
| `name`             | string  | Material asset name                                                      |
| `asset_path`       | string  | Full object path                                                         |
| `class`            | string  | UClass name (`Material`, `MaterialInstanceConstant`, etc.)               |
| `material_type`    | string  | Friendly type: `Material`, `MaterialInstanceConstant`, `MaterialInstanceDynamic`, `MaterialInstance`, `MaterialInterface` |
| `parent`           | string  | Parent material path (material instances only)                           |
| `base_material`    | string  | Ultimate base material path (resolved through the parent chain)          |
| `domain`           | string  | `Surface`, `DeferredDecal`, `LightFunction`, `Volume`, `PostProcess`, `UI`, `RuntimeVirtualTexture` |
| `blend_mode`       | string  | `Opaque`, `Masked`, `Translucent`, `Additive`, `Modulate`, `AlphaComposite`, `AlphaHoldout` |
| `shading_model`    | string  | `DefaultLit`, `Unlit`, `Subsurface`, `ClearCoat`, etc.                  |
| `two_sided`        | bool    | Whether the material renders both faces                                  |
| `is_masked`        | bool    | Whether the material uses an opacity mask                                |
| `is_translucent`   | bool    | Whether the blend mode is non-opaque (Translucent, Additive, etc.)      |
| `parameters`       | object  | Contains `scalar`, `vector`, `texture`, `static_switch` arrays          |
| `expression_count` | number  | Number of expression nodes (base materials only, with `include_expressions`) |
| `expressions`      | array   | Expression node details (base materials only, with `include_expressions`) |

### Parameter Fields

| Field         | Type          | Description                                          |
| ------------- | ------------- | ---------------------------------------------------- |
| `name`        | string        | Parameter name                                       |
| `value`       | varies        | Current value (number, color object, texture path, or bool) |
| `group`       | string        | Parameter group (if set)                             |
| `description` | string        | Parameter description (if set)                       |
| `overridden`  | bool          | Whether this parameter overrides the parent (instances only) |
| `id`          | string        | Parameter GUID                                       |

### Expression Fields

| Field       | Type   | Description                                    |
| ----------- | ------ | ---------------------------------------------- |
| `class`     | string | Expression class (e.g. `MaterialExpressionTextureSample`) |
| `caption`   | string | Display caption from the graph editor          |
| `description` | string | User description (if set)                   |
| `editor_x`  | number | X position in the material graph editor       |
| `editor_y`  | number | Y position in the material graph editor       |
| `id`        | string | Expression GUID (if valid)                     |

## Errors

| Condition            | `isError` | Message                                  |
| -------------------- | --------- | ---------------------------------------- |
| Missing `asset_path` | `true`    | `Missing required parameter 'asset_path'` |
| Asset not found      | `true`    | `Material not found: '/Game/...'`        |
