# set_control_rig_element_property

Set properties on a Control Rig hierarchy element: transform, parent, and control-specific settings (shape, color, display name).

## Input

```json
{
  "asset_path": "/Game/Rigs/CR_MyRig.CR_MyRig",
  "element_name": "IK_Hand_R",
  "element_type": "Control",
  "transform": { "location": { "x": 100, "y": 50, "z": 0 } },
  "shape_visible": true,
  "shape_color": { "r": 0, "g": 1, "b": 0, "a": 1 },
  "display_name": "Right Hand IK"
}
```

|| Parameter       | Required | Default | Description                                        |
|| --------------- | -------- | ------- | -------------------------------------------------- |
|| `asset_path`    | **yes**  | ---     | Content path to the Control Rig Blueprint.         |
|| `element_name`  | **yes**  | ---     | Name of the element to modify.                     |
|| `element_type`  | **yes**  | ---     | Type: `"Bone"`, `"Control"`, `"Null"`, `"Curve"`, `"Connector"`. |
|| `transform`     | no       | ---     | New local transform (location, rotation, scale).   |
|| `parent_name`   | no       | ---     | Re-parent to this element.                         |
|| `parent_type`   | no       | ---     | Type of the new parent element.                    |
|| `shape_visible` | no       | ---     | For Controls: set shape visibility.                |
|| `shape_color`   | no       | ---     | For Controls: RGBA shape color.                    |
|| `display_name`  | no       | ---     | For Controls: display name.                        |

At least one property must be provided.

## Output

|| Field            | Type   | Description                          |
|| ---------------- | ------ | ------------------------------------ |
|| `element_name`   | string | Name of the modified element         |
|| `element_type`   | string | Type of the modified element         |
|| `properties_set` | number | Number of properties changed         |
|| `values`         | object | Key-value pairs of properties set    |

## Examples

### Set transform

```json
{
  "asset_path": "/Game/Rigs/CR_MyRig.CR_MyRig",
  "element_name": "CustomBone",
  "element_type": "Bone",
  "transform": { "location": { "x": 50, "y": 0, "z": 100 } }
}
```

### Change control color

```json
{
  "asset_path": "/Game/Rigs/CR_MyRig.CR_MyRig",
  "element_name": "IK_Foot_L",
  "element_type": "Control",
  "shape_color": { "r": 0, "g": 0, "b": 1, "a": 1 }
}
```

## Errors

|| Condition               | `isError` | Message                                    |
|| ----------------------- | --------- | ------------------------------------------ |
|| Missing `asset_path`    | true      | 'asset_path' is required                   |
|| Missing `element_name`  | true      | 'element_name' is required                 |
|| Missing `element_type`  | true      | 'element_type' is required                 |
|| Element not found       | true      | Element not found: ...                     |
|| No properties provided  | true      | No properties were set ...                 |
|| Parent not found        | true      | Parent element not found: ...              |
|| Asset not found         | true      | Control Rig Blueprint not found or wrong type: ... |
