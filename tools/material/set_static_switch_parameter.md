# set_static_switch_parameter

Set a static switch parameter on a `MaterialInstanceConstant`. Static switches control shader permutation selection and require `UpdateStaticPermutation()` — a fundamentally different API path from scalar/vector/texture parameters. The instance is saved to disk after modification.

## Input

```json
{
  "asset_path": "/Game/Materials/MI_MyInstance",
  "parameter_name": "UseDetail",
  "value": true
}
```

| Parameter        | Required | Description                                                              |
| ---------------- | -------- | ------------------------------------------------------------------------ |
| `asset_path`     | **yes**  | Content path to the material instance (e.g. `/Game/Materials/MI_MyInst`) |
| `parameter_name` | **yes**  | Static switch parameter name                                             |
| `value`          | **yes**  | `true` to enable the switch, `false` to disable                          |

## Output

```json
{
  "name": "MI_MyInstance",
  "asset_path": "/Game/Materials",
  "parameter_name": "UseDetail",
  "value": true,
  "was_existing_override": false
}
```

| Field                  | Type    | Description                                                       |
| ---------------------- | ------- | ----------------------------------------------------------------- |
| `name`                 | string  | Material instance asset name                                      |
| `asset_path`           | string  | Package path of the material instance                             |
| `parameter_name`       | string  | The parameter that was set                                        |
| `value`                | boolean | The value that was applied                                        |
| `was_existing_override`| boolean | `true` if the parameter was already overridden; `false` if newly added |

## Examples

### Enable a static switch

```json
{
  "asset_path": "/Game/Materials/MI_Ground",
  "parameter_name": "UseDetail",
  "value": true
}
```

Response:

```json
{
  "name": "MI_Ground",
  "asset_path": "/Game/Materials",
  "parameter_name": "UseDetail",
  "value": true,
  "was_existing_override": false
}
```

### Disable a previously enabled switch

```json
{
  "asset_path": "/Game/Materials/MI_Ground",
  "parameter_name": "UseDetail",
  "value": false
}
```

Response:

```json
{
  "name": "MI_Ground",
  "asset_path": "/Game/Materials",
  "parameter_name": "UseDetail",
  "value": false,
  "was_existing_override": true
}
```

## Notes

- Only works on `UMaterialInstanceConstant` assets — base `UMaterial` assets are not supported
- If the named parameter does not yet exist as an override, it is added as a new override entry
- Static switch changes trigger `UpdateStaticPermutation()` and `InitStaticPermutation()` to rebuild the shader permutation
- The material instance is saved to disk after modification

## Errors

| Condition                          | `isError` | Message                                                          |
| ---------------------------------- | --------- | ---------------------------------------------------------------- |
| Missing `asset_path`               | `true`    | `Missing required parameter 'asset_path'`                        |
| Missing `parameter_name`           | `true`    | `Missing required parameter 'parameter_name'`                    |
| Missing `value`                    | `true`    | `Missing required parameter 'value'`                             |
| Asset not found                    | `true`    | `Material instance not found: '...'`                             |
| Asset is a base material           | `true`    | `Asset '...' is not a MaterialInstanceConstant`                  |
