# disconnect_material_expression

Break a connection at a material input pin or an expression's input. When targeting a material pin, the connection feeding that pin is removed. When targeting an expression input, the connection at the specified `input_index` is broken. The material is saved and recompiled after disconnection. Only works on base materials (UMaterial), not material instances.

## Input

```json
{
  "asset_path": "/Game/Materials/M_MyMaterial",
  "material_pin": "BaseColor"
}
```

| Parameter          | Required                               | Description                                                                   |
| ------------------ | -------------------------------------- | ----------------------------------------------------------------------------- |
| `asset_path`       | **yes**                                | Content path to the base material asset (e.g. `/Game/Materials/M_MyMaterial`) |
| `material_pin`     | **yes** (or `expression_id` / `expression_index`) | Name of the material input pin to disconnect (see list below)                 |
| `expression_id`    | **yes** (or `material_pin`)            | GUID of the expression whose input should be disconnected                     |
| `expression_index` | **yes** (or `material_pin`)            | Zero-based index of the expression whose input should be disconnected         |
| `input_index`      | no                                     | Input index to disconnect on the target expression (default `0`); ignored when using `material_pin` |

### Material Pin Values

`BaseColor`, `Metallic`, `Specular`, `Roughness`, `Normal`, `EmissiveColor`, `Opacity`, `OpacityMask`, `WorldPositionOffset`, `AmbientOcclusion`, `Refraction`, `SubsurfaceColor`, `Anisotropy`, `Tangent`

## Output

```json
{
  "disconnected": true,
  "material_name": "M_MyMaterial",
  "asset_path": "/Game/Materials/M_MyMaterial.M_MyMaterial",
  "target": "BaseColor"
}
```

| Field           | Type    | Description                                                                               |
| --------------- | ------- | ----------------------------------------------------------------------------------------- |
| `disconnected`  | boolean | Always `true` on success                                                                  |
| `material_name` | string  | Material asset name                                                                       |
| `asset_path`    | string  | Full asset path                                                                           |
| `target`        | string  | The material pin name or a description of the expression input (e.g. `Multiply:0`)        |

## Examples

### Disconnect a material pin

```json
{
  "asset_path": "/Game/Materials/M_Ground",
  "material_pin": "BaseColor"
}
```

Response:

```json
{
  "disconnected": true,
  "material_name": "M_Ground",
  "asset_path": "/Game/Materials/M_Ground.M_Ground",
  "target": "BaseColor"
}
```

### Disconnect an expression input by GUID

```json
{
  "asset_path": "/Game/Materials/M_Ground",
  "expression_id": "DEADBEEF-1234-5678-9ABC-DEF012345678",
  "input_index": 1
}
```

Response:

```json
{
  "disconnected": true,
  "material_name": "M_Ground",
  "asset_path": "/Game/Materials/M_Ground.M_Ground",
  "target": "Multiply:1"
}
```

## Notes

- Only works on base `UMaterial` assets — material instances (`UMaterialInstanceConstant`) are not supported
- Exactly one target must be specified: either `material_pin` or `expression_id` / `expression_index` — not both
- If both `expression_id` and `expression_index` are provided, `expression_id` takes precedence
- `input_index` selects which input on the target expression to disconnect (e.g. `Multiply` input 0 = A, input 1 = B)
- The material is saved to disk and recompiled after the connection is broken

## Errors

| Condition                          | `isError` | Message                                                                          |
| ---------------------------------- | --------- | -------------------------------------------------------------------------------- |
| Missing `asset_path`               | `true`    | `Missing required parameter 'asset_path'`                                        |
| Missing target                     | `true`    | `Missing required parameter 'material_pin', 'expression_id', or 'expression_index'` |
| Unknown material pin               | `true`    | `Unknown material pin: '...'`                                                    |
| Pin not connected                  | `true`    | `Material pin '...' is not connected`                                            |
| Expression not found               | `true`    | `Expression not found: '...'`                                                    |
| Material not found                 | `true`    | `Material not found: '...'`                                                      |
| Asset is a material instance       | `true`    | `Asset is a Material Instance, not a base Material: '...'`                       |
