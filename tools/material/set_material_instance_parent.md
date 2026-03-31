# set_material_instance_parent

Change the parent material of an existing `MaterialInstanceConstant`. The new parent can be a base `UMaterial` or another `UMaterialInstanceConstant`. The instance is saved to disk after modification.

## Input

```json
{
  "asset_path": "/Game/Materials/MI_MyInstance",
  "parent": "/Game/Materials/M_NewParent"
}
```

| Parameter    | Required | Description                                                              |
| ------------ | -------- | ------------------------------------------------------------------------ |
| `asset_path` | **yes**  | Content path to the material instance to modify                          |
| `parent`     | **yes**  | Content path to the new parent material or material instance             |

## Output

```json
{
  "name": "MI_MyInstance",
  "asset_path": "/Game/Materials",
  "old_parent": "/Game/Materials/M_OldParent.M_OldParent",
  "new_parent": "/Game/Materials/M_NewParent.M_NewParent"
}
```

| Field        | Type   | Description                              |
| ------------ | ------ | ---------------------------------------- |
| `name`       | string | Material instance asset name             |
| `asset_path` | string | Package path of the material instance    |
| `old_parent` | string | Full path of the previous parent         |
| `new_parent` | string | Full path of the newly assigned parent   |

## Examples

### Change parent to a different base material

```json
{
  "asset_path": "/Game/Materials/MI_Ground",
  "parent": "/Game/Materials/M_Rock"
}
```

Response:

```json
{
  "name": "MI_Ground",
  "asset_path": "/Game/Materials",
  "old_parent": "/Game/Materials/M_Grass.M_Grass",
  "new_parent": "/Game/Materials/M_Rock.M_Rock"
}
```

## Notes

- Only works on `UMaterialInstanceConstant` assets — base `UMaterial` assets cannot be reparented
- The new parent can be either a base material or another material instance
- Self-parenting (setting the instance as its own parent) is rejected
- The material instance is saved to disk after modification

## Errors

| Condition                          | `isError` | Message                                                          |
| ---------------------------------- | --------- | ---------------------------------------------------------------- |
| Missing `asset_path`               | `true`    | `Missing required parameter 'asset_path'`                        |
| Missing `parent`                   | `true`    | `Missing required parameter 'parent'`                            |
| Instance not found                 | `true`    | `Material instance not found: '...'`                             |
| Asset is a base material           | `true`    | `Asset '...' is not a MaterialInstanceConstant`                  |
| Parent not found                   | `true`    | `Parent material not found: '...'`                               |
| Self-parenting                     | `true`    | `Cannot set a material instance as its own parent`               |
