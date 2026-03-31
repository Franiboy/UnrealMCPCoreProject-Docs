# remove_material_expression

Remove a material expression node from a base UMaterial asset's graph. The target expression can be identified by its GUID (`expression_id`) or its zero-based array index (`expression_index`). Any connections involving the removed expression are automatically broken. The material is saved and recompiled after removal. Only works on base materials (UMaterial), not material instances.

## Input

```json
{
  "asset_path": "/Game/Materials/M_MyMaterial",
  "expression_id": "A1B2C3D4-E5F6-7890-ABCD-EF1234567890"
}
```

| Parameter          | Required                       | Description                                                                   |
| ------------------ | ------------------------------ | ----------------------------------------------------------------------------- |
| `asset_path`       | **yes**                        | Content path to the base material asset (e.g. `/Game/Materials/M_MyMaterial`) |
| `expression_id`    | **yes** (or `expression_index`) | GUID string identifying the expression to remove                              |
| `expression_index` | **yes** (or `expression_id`)    | Zero-based index of the expression in the material's array                    |

## Output

```json
{
  "removed": true,
  "material_name": "M_MyMaterial",
  "asset_path": "/Game/Materials/M_MyMaterial.M_MyMaterial",
  "removed_class": "MaterialExpressionMultiply",
  "removed_id": "A1B2C3D4-E5F6-7890-ABCD-EF1234567890"
}
```

| Field           | Type    | Description                                          |
| --------------- | ------- | ---------------------------------------------------- |
| `removed`       | boolean | Always `true` on success                             |
| `material_name` | string  | Material asset name                                  |
| `asset_path`    | string  | Full asset path                                      |
| `removed_class` | string  | Class name of the removed expression                 |
| `removed_id`    | string  | GUID of the removed expression                       |

## Examples

### Remove by GUID

```json
{
  "asset_path": "/Game/Materials/M_Ground",
  "expression_id": "1A2B3C4D-5E6F-7890-ABCD-EF1234567890"
}
```

Response:

```json
{
  "removed": true,
  "material_name": "M_Ground",
  "asset_path": "/Game/Materials/M_Ground.M_Ground",
  "removed_class": "MaterialExpressionConstant",
  "removed_id": "1A2B3C4D-5E6F-7890-ABCD-EF1234567890"
}
```

### Remove by index

```json
{
  "asset_path": "/Game/Materials/M_Ground",
  "expression_index": 2
}
```

Response:

```json
{
  "removed": true,
  "material_name": "M_Ground",
  "asset_path": "/Game/Materials/M_Ground.M_Ground",
  "removed_class": "MaterialExpressionAdd",
  "removed_id": "DEADBEEF-1234-5678-9ABC-DEF012345678"
}
```

## Notes

- Only works on base `UMaterial` assets — material instances (`UMaterialInstanceConstant`) are not supported
- If both `expression_id` and `expression_index` are provided, `expression_id` takes precedence
- All connections to and from the removed expression are automatically broken before removal
- The material is saved to disk and recompiled after the expression is removed
- After removal, the indices of remaining expressions may shift — prefer using GUIDs for stable identification

## Errors

| Condition                          | `isError` | Message                                                    |
| ---------------------------------- | --------- | ---------------------------------------------------------- |
| Missing `asset_path`               | `true`    | `Missing required parameter 'asset_path'`                  |
| Missing identifier                 | `true`    | `Missing required parameter 'expression_id' or 'expression_index'` |
| Expression not found               | `true`    | `Expression not found: '...'`                              |
| Material not found                 | `true`    | `Material not found: '...'`                                |
| Asset is a material instance       | `true`    | `Asset is a Material Instance, not a base Material: '...'` |
