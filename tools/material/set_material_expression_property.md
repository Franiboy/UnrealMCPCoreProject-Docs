# set_material_expression_property

Set a property on a material expression node in a base UMaterial asset via Unreal's reflection system. The target expression can be identified by its GUID (`expression_id`) or its zero-based array index (`expression_index`). Supports float, int, bool, string, name, byte, enum, and struct property types (FLinearColor, FColor, FVector, FVector2D, FVector4, FRotator). Struct properties accept JSON objects or Unreal text format strings. Dot-notation (e.g. `DefaultValue.R`) is supported for navigating into struct sub-properties. The material is saved and recompiled after modification. Only works on base materials (UMaterial), not material instances.

## Input

```json
{
  "asset_path": "/Game/Materials/M_MyMaterial",
  "expression_id": "A1B2C3D4-E5F6-7890-ABCD-EF1234567890",
  "property_name": "R",
  "property_value": 0.5
}
```

| Parameter          | Required                       | Description                                                                   |
| ------------------ | ------------------------------ | ----------------------------------------------------------------------------- |
| `asset_path`       | **yes**                        | Content path to the base material asset (e.g. `/Game/Materials/M_MyMaterial`) |
| `expression_id`    | **yes** (or `expression_index`) | GUID string identifying the target expression                                 |
| `expression_index` | **yes** (or `expression_id`)    | Zero-based index of the expression in the material's array                    |
| `property_name`    | **yes**                        | Name of the property to set — supports dot-notation for struct sub-properties (e.g. `DefaultValue.R`) |
| `property_value`   | **yes**                        | Value to assign — type must be compatible with the property. Struct types accept JSON objects (e.g. `{"R": 0.5, "G": 0.3, "B": 0.1, "A": 1.0}`) or Unreal text format strings |

## Output

```json
{
  "material_name": "M_MyMaterial",
  "asset_path": "/Game/Materials/M_MyMaterial.M_MyMaterial",
  "expression_class": "MaterialExpressionConstant",
  "property_name": "R",
  "property_value": 0.5,
  "expression_id": "A1B2C3D4-E5F6-7890-ABCD-EF1234567890"
}
```

| Field              | Type   | Description                                         |
| ------------------ | ------ | --------------------------------------------------- |
| `material_name`    | string | Material asset name                                 |
| `asset_path`       | string | Full asset path                                     |
| `expression_class` | string | Class name of the modified expression               |
| `property_name`    | string | The property that was set                           |
| `property_value`   | any    | The value that was applied                          |
| `expression_id`    | string | GUID of the modified expression                     |

## Examples

### Set the value of a Constant node

```json
{
  "asset_path": "/Game/Materials/M_Ground",
  "expression_id": "1A2B3C4D-5E6F-7890-ABCD-EF1234567890",
  "property_name": "R",
  "property_value": 0.75
}
```

Response:

```json
{
  "material_name": "M_Ground",
  "asset_path": "/Game/Materials/M_Ground.M_Ground",
  "expression_class": "MaterialExpressionConstant",
  "property_name": "R",
  "property_value": 0.75,
  "expression_id": "1A2B3C4D-5E6F-7890-ABCD-EF1234567890"
}
```

### Set a boolean property by index

```json
{
  "asset_path": "/Game/Materials/M_Ground",
  "expression_index": 0,
  "property_name": "bCollapsed",
  "property_value": true
}
```

Response:

```json
{
  "material_name": "M_Ground",
  "asset_path": "/Game/Materials/M_Ground.M_Ground",
  "expression_class": "MaterialExpressionTextureSample",
  "property_name": "bCollapsed",
  "property_value": true,
  "expression_id": "DEADBEEF-1234-5678-9ABC-DEF012345678"
}
```

### Set FLinearColor via JSON object (VectorParameter default)

```json
{
  "asset_path": "/Game/Materials/M_Ground",
  "expression_index": 2,
  "property_name": "DefaultValue",
  "property_value": {"R": 0.1, "G": 0.5, "B": 0.0, "A": 1.0}
}
```

Response:

```json
{
  "material_name": "M_Ground",
  "asset_path": "/Game/Materials/M_Ground.M_Ground",
  "expression_class": "MaterialExpressionVectorParameter",
  "property_name": "DefaultValue",
  "property_value": "(R=0.1000,G=0.5000,B=0.0000,A=1.0000)",
  "expression_id": "CAFEBABE-1234-5678-9ABC-DEF012345678"
}
```

### Set a single color channel via dot-notation

```json
{
  "asset_path": "/Game/Materials/M_Ground",
  "expression_index": 2,
  "property_name": "DefaultValue.R",
  "property_value": 0.99
}
```

Response:

```json
{
  "material_name": "M_Ground",
  "asset_path": "/Game/Materials/M_Ground.M_Ground",
  "expression_class": "MaterialExpressionVectorParameter",
  "property_name": "DefaultValue.R",
  "property_value": "0.99",
  "expression_id": "CAFEBABE-1234-5678-9ABC-DEF012345678"
}
```

## Notes

- Only works on base `UMaterial` assets — material instances (`UMaterialInstanceConstant`) are not supported
- If both `expression_id` and `expression_index` are provided, `expression_id` takes precedence
- Property resolution uses Unreal's reflection system (`FProperty`), so the property name must match exactly
- Dot-notation (e.g. `DefaultValue.R`) is supported for navigating into struct sub-properties
- Supported property types: `float`, `int`, `bool`, `string`, `name`, `byte`, `enum`
- Supported struct types: `FLinearColor`, `FColor`, `FVector`, `FVector2D`, `FVector4`, `FRotator` — accepts JSON objects or Unreal text format strings
- Other struct types fall back to `ImportText` with a string value
- The material is saved to disk and recompiled after the property is set
- Common property names by expression type:
  - `Constant` → `R`
  - `Constant3Vector` → `Constant` (FLinearColor)
  - `TextureSample` → `Texture`
  - `ScalarParameter` → `DefaultValue`, `ParameterName`

## Errors

| Condition                          | `isError` | Message                                                                      |
| ---------------------------------- | --------- | ---------------------------------------------------------------------------- |
| Missing `asset_path`               | `true`    | `Missing required parameter 'asset_path'`                                    |
| Missing identifier                 | `true`    | `Missing required parameter 'expression_id' or 'expression_index'`           |
| Missing `property_name`            | `true`    | `Missing required parameter 'property_name'`                                 |
| Missing `property_value`           | `true`    | `Missing required parameter 'property_value'`                                |
| Expression not found               | `true`    | `Expression not found: '...'`                                                |
| Property not found                 | `true`    | `Property '...' not found on expression '...'`                               |
| Unsupported property type          | `true`    | `Unsupported property type '...' for property '...'`                         |
| Material not found                 | `true`    | `Material not found: '...'`                                                  |
| Asset is a material instance       | `true`    | `Asset is a Material Instance, not a base Material: '...'`                   |
