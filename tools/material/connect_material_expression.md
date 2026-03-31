# connect_material_expression

Wire a material expression output to either a material input pin (e.g. BaseColor, Roughness) or another expression's input. The source and target expressions are identified by GUID or zero-based index. The material is saved and recompiled after the connection is made. Only works on base materials (UMaterial), not material instances.

## Input

```json
{
  "asset_path": "/Game/Materials/M_MyMaterial",
  "source_id": "A1B2C3D4-E5F6-7890-ABCD-EF1234567890",
  "material_pin": "BaseColor",
  "output_index": 0
}
```

| Parameter        | Required                           | Description                                                                   |
| ---------------- | ---------------------------------- | ----------------------------------------------------------------------------- |
| `asset_path`     | **yes**                            | Content path to the base material asset (e.g. `/Game/Materials/M_MyMaterial`) |
| `source_id`      | **yes** (or `source_index`)        | GUID of the source expression providing the output                            |
| `source_index`   | **yes** (or `source_id`)           | Zero-based index of the source expression                                     |
| `material_pin`   | **yes** (or `target_id` / `target_index`) | Name of the material input pin to connect to (see list below)                 |
| `target_id`      | **yes** (or `material_pin`)        | GUID of the target expression to connect into                                 |
| `target_index`   | **yes** (or `material_pin`)        | Zero-based index of the target expression                                     |
| `output_index`   | no                                 | Output index on the source expression (default `0`)                           |
| `input_index`    | no                                 | Input index on the target expression (default `0`); ignored when using `material_pin` |

### Material Pin Values

`BaseColor`, `Metallic`, `Specular`, `Roughness`, `Normal`, `EmissiveColor`, `Opacity`, `OpacityMask`, `WorldPositionOffset`, `AmbientOcclusion`, `Refraction`, `SubsurfaceColor`, `Anisotropy`, `Tangent`

## Output

```json
{
  "connected": true,
  "material_name": "M_MyMaterial",
  "asset_path": "/Game/Materials/M_MyMaterial.M_MyMaterial",
  "source_class": "MaterialExpressionConstant3Vector",
  "output_index": 0,
  "target": "BaseColor"
}
```

| Field           | Type    | Description                                                                              |
| --------------- | ------- | ---------------------------------------------------------------------------------------- |
| `connected`     | boolean | Always `true` on success                                                                 |
| `material_name` | string  | Material asset name                                                                      |
| `asset_path`    | string  | Full asset path                                                                          |
| `source_class`  | string  | Class name of the source expression                                                      |
| `output_index`  | integer | Output index used on the source expression                                               |
| `target`        | string  | The material pin name or a description of the target expression input (e.g. `Multiply:0`) |

## Examples

### Connect an expression to a material pin

```json
{
  "asset_path": "/Game/Materials/M_Ground",
  "source_id": "1A2B3C4D-5E6F-7890-ABCD-EF1234567890",
  "material_pin": "BaseColor"
}
```

Response:

```json
{
  "connected": true,
  "material_name": "M_Ground",
  "asset_path": "/Game/Materials/M_Ground.M_Ground",
  "source_class": "MaterialExpressionConstant3Vector",
  "output_index": 0,
  "target": "BaseColor"
}
```

### Connect one expression to another expression's input

```json
{
  "asset_path": "/Game/Materials/M_Ground",
  "source_index": 0,
  "target_index": 2,
  "output_index": 0,
  "input_index": 1
}
```

Response:

```json
{
  "connected": true,
  "material_name": "M_Ground",
  "asset_path": "/Game/Materials/M_Ground.M_Ground",
  "source_class": "MaterialExpressionConstant",
  "output_index": 0,
  "target": "Multiply:1"
}
```

## Notes

- Only works on base `UMaterial` assets — material instances (`UMaterialInstanceConstant`) are not supported
- Exactly one target must be specified: either `material_pin` or `target_id` / `target_index` — not both
- If both `source_id` and `source_index` are provided, `source_id` takes precedence; same for target identifiers
- `output_index` selects which output of a multi-output expression to use (e.g. `BreakOutFloat2Components` has outputs 0 and 1)
- `input_index` selects which input on the target expression to connect to (e.g. `Multiply` input 0 = A, input 1 = B)
- The material is saved to disk and recompiled after the connection is made

## Errors

| Condition                          | `isError` | Message                                                                      |
| ---------------------------------- | --------- | ---------------------------------------------------------------------------- |
| Missing `asset_path`               | `true`    | `Missing required parameter 'asset_path'`                                    |
| Missing source identifier          | `true`    | `Missing required parameter 'source_id' or 'source_index'`                   |
| Missing target                     | `true`    | `Missing required parameter 'material_pin', 'target_id', or 'target_index'`  |
| Source expression not found        | `true`    | `Source expression not found: '...'`                                         |
| Target expression not found        | `true`    | `Target expression not found: '...'`                                         |
| Unknown material pin               | `true`    | `Unknown material pin: '...'`                                                |
| Material not found                 | `true`    | `Material not found: '...'`                                                  |
| Asset is a material instance       | `true`    | `Asset is a Material Instance, not a base Material: '...'`                   |
