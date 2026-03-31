# remove_control_rig_element

Remove an element from a Control Rig's hierarchy by name and type.

## Input

```json
{
  "asset_path": "/Game/Rigs/CR_MyRig.CR_MyRig",
  "element_name": "IK_Hand_R",
  "element_type": "Control"
}
```

|| Parameter      | Required | Default | Description                                            |
|| -------------- | -------- | ------- | ------------------------------------------------------ |
|| `asset_path`   | **yes**  | ---     | Content path to the Control Rig Blueprint.             |
|| `element_name` | **yes**  | ---     | Name of the element to remove.                         |
|| `element_type` | **yes**  | ---     | Type: `"Bone"`, `"Control"`, `"Null"`, `"Curve"`, `"Connector"`. |

## Output

|| Field          | Type    | Description                           |
|| -------------- | ------- | ------------------------------------- |
|| `element_name` | string  | Name of the removed element           |
|| `element_type` | string  | Type of the removed element           |
|| `removed`      | boolean | `true` if successfully removed        |

## Examples

### Remove a control

```json
{
  "asset_path": "/Game/Rigs/CR_MyRig.CR_MyRig",
  "element_name": "IK_Hand_R",
  "element_type": "Control"
}
```

## Errors

|| Condition              | `isError` | Message                                    |
|| ---------------------- | --------- | ------------------------------------------ |
|| Missing `asset_path`   | true      | 'asset_path' is required                   |
|| Missing `element_name` | true      | 'element_name' is required                 |
|| Missing `element_type` | true      | 'element_type' is required                 |
|| Element not found      | true      | Element not found: ...                     |
|| Asset not found        | true      | Control Rig Blueprint not found or wrong type: ... |
|| Removal failed         | true      | Failed to remove element: ...              |
