# add_material_expression

Add a material expression node to a base UMaterial asset's graph. The `MaterialExpression` prefix is added automatically to the provided class name — for example, passing `Multiply` resolves to `UMaterialExpressionMultiply`. The material is saved and recompiled after the expression is added. Only works on base materials (UMaterial), not material instances.

## Input

```json
{
  "asset_path": "/Game/Materials/M_MyMaterial",
  "expression_class": "Multiply",
  "editor_x": -400,
  "editor_y": 200
}
```

| Parameter          | Required | Description                                                                                        |
| ------------------ | -------- | -------------------------------------------------------------------------------------------------- |
| `asset_path`       | **yes**  | Content path to the base material asset (e.g. `/Game/Materials/M_MyMaterial`)                      |
| `expression_class` | **yes**  | Expression class name without the `MaterialExpression` prefix (e.g. `Constant`, `Multiply`, `Add`) |
| `editor_x`         | no       | Horizontal position in the material editor graph (default `0`)                                     |
| `editor_y`         | no       | Vertical position in the material editor graph (default `0`)                                       |

## Output

```json
{
  "class": "MaterialExpressionMultiply",
  "caption": "Multiply",
  "material_name": "M_MyMaterial",
  "asset_path": "/Game/Materials/M_MyMaterial.M_MyMaterial",
  "id": "A1B2C3D4-E5F6-7890-ABCD-EF1234567890",
  "index": 3,
  "editor_x": -400,
  "editor_y": 200
}
```

| Field           | Type    | Description                                                    |
| --------------- | ------- | -------------------------------------------------------------- |
| `class`         | string  | Full expression class name (e.g. `MaterialExpressionMultiply`) |
| `caption`       | string  | Display caption shown in the material editor                   |
| `material_name` | string  | Material asset name                                            |
| `asset_path`    | string  | Full asset path                                                |
| `id`            | string  | GUID identifier for the newly created expression               |
| `index`         | integer | Zero-based index of the expression in the material's array     |
| `editor_x`      | integer | Horizontal position in the editor graph                        |
| `editor_y`      | integer | Vertical position in the editor graph                          |

## Examples

### Add a Constant node at the default position

```json
{
  "asset_path": "/Game/Materials/M_Ground",
  "expression_class": "Constant"
}
```

Response:

```json
{
  "class": "MaterialExpressionConstant",
  "caption": "Constant",
  "material_name": "M_Ground",
  "asset_path": "/Game/Materials/M_Ground.M_Ground",
  "id": "1A2B3C4D-5E6F-7890-ABCD-EF1234567890",
  "index": 0,
  "editor_x": 0,
  "editor_y": 0
}
```

### Add a Multiply node at a specific position

```json
{
  "asset_path": "/Game/Materials/M_Ground",
  "expression_class": "Multiply",
  "editor_x": -600,
  "editor_y": 300
}
```

Response:

```json
{
  "class": "MaterialExpressionMultiply",
  "caption": "Multiply",
  "material_name": "M_Ground",
  "asset_path": "/Game/Materials/M_Ground.M_Ground",
  "id": "DEADBEEF-1234-5678-9ABC-DEF012345678",
  "index": 1,
  "editor_x": -600,
  "editor_y": 300
}
```

## Notes

- Only works on base `UMaterial` assets — material instances (`UMaterialInstanceConstant`) are not supported
- The `MaterialExpression` prefix is added automatically; pass just `Constant`, not `MaterialExpressionConstant`
- The material is saved to disk and recompiled after the expression is added
- Use `editor_x` / `editor_y` to lay out nodes visually in the material editor graph
- The returned `id` (GUID) can be used in subsequent calls to `connect_material_expression`, `set_material_expression_property`, and `remove_material_expression`

## Errors

| Condition                          | `isError` | Message                                                    |
| ---------------------------------- | --------- | ---------------------------------------------------------- |
| Missing `asset_path`               | `true`    | `Missing required parameter 'asset_path'`                  |
| Missing `expression_class`         | `true`    | `Missing required parameter 'expression_class'`            |
| Expression class not found         | `true`    | `Expression class not found: '...'`                        |
| Material not found                 | `true`    | `Material not found: '...'`                                |
| Asset is a material instance       | `true`    | `Asset is a Material Instance, not a base Material: '...'` |
