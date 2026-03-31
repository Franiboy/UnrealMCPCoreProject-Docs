# compile_material

Force recompile a base UMaterial asset and return compilation status. Reports success, any errors, and saves the material to disk after compilation. Only works on base materials (UMaterial), not material instances.

## Input

```json
{
  "asset_path": "/Game/Materials/M_MyMaterial"
}
```

| Parameter    | Required | Description                                                                   |
| ------------ | -------- | ----------------------------------------------------------------------------- |
| `asset_path` | **yes**  | Content path to the base material asset (e.g. `/Game/Materials/M_MyMaterial`) |

## Output

```json
{
  "name": "M_MyMaterial",
  "asset_path": "/Game/Materials/M_MyMaterial.M_MyMaterial",
  "class": "Material",
  "success": true,
  "errors": [],
  "error_count": 0
}
```

| Field        | Type    | Description                                    |
| ------------ | ------- | ---------------------------------------------- |
| `name`       | string  | Material asset name                            |
| `asset_path` | string  | Full asset path                                |
| `class`      | string  | Always `Material`                              |
| `success`    | boolean | `true` if compilation completed with no errors |
| `errors`     | array   | Array of error strings (empty if successful)   |
| `error_count`| integer | Number of compilation errors                   |

## Examples

### Compile a material with no errors

```json
{
  "asset_path": "/Game/Materials/M_Ground"
}
```

Response:

```json
{
  "name": "M_Ground",
  "asset_path": "/Game/Materials/M_Ground.M_Ground",
  "class": "Material",
  "success": true,
  "errors": [],
  "error_count": 0
}
```

### Compile a material with errors

```json
{
  "asset_path": "/Game/Materials/M_Broken"
}
```

Response:

```json
{
  "name": "M_Broken",
  "asset_path": "/Game/Materials/M_Broken.M_Broken",
  "class": "Material",
  "success": false,
  "errors": [
    "Material expression has no valid output"
  ],
  "error_count": 1
}
```

## Notes

- Only works on base `UMaterial` assets — material instances (`UMaterialInstanceConstant`) cannot be compiled directly
- The material is saved to disk after compilation regardless of success or failure
- Use this after modifying material expressions or properties to check for errors before use

## Errors

| Condition                          | `isError` | Message                                                    |
| ---------------------------------- | --------- | ---------------------------------------------------------- |
| Missing `asset_path`               | `true`    | `Missing required parameter 'asset_path'`                  |
| Material not found                 | `true`    | `Material not found: '...'`                                |
| Asset is a material instance       | `true`    | `Asset is a Material Instance, not a base Material: '...'` |
