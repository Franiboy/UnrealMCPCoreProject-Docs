# add_control_rig_element

Add an element (Bone, Control, Null, or Curve) to a Control Rig's hierarchy.

## Input

```json
{
  "asset_path": "/Game/Rigs/CR_MyRig.CR_MyRig",
  "element_name": "IK_Hand_R",
  "element_type": "Control",
  "parent_name": "Hand_R",
  "parent_type": "Bone",
  "control_type": "Position",
  "shape_visible": true,
  "shape_color": { "r": 1, "g": 0, "b": 0, "a": 1 },
  "transform": { "location": { "x": 0, "y": 0, "z": 0 } }
}
```

|| Parameter       | Required | Default          | Description                                            |
|| --------------- | -------- | ---------------- | ------------------------------------------------------ |
|| `asset_path`    | **yes**  | ---              | Content path to the Control Rig Blueprint.             |
|| `element_name`  | **yes**  | ---              | Name for the new element.                              |
|| `element_type`  | **yes**  | ---              | Type: `"Bone"`, `"Control"`, `"Null"`, or `"Curve"`.   |
|| `parent_name`   | no       | ---              | Parent element name.                                   |
|| `parent_type`   | no       | same as element  | Parent element type.                                   |
|| `transform`     | no       | identity         | Initial transform (location, rotation, scale).         |
|| `control_type`  | no       | `"EulerTransform"` | For Controls: value type (`"Float"`, `"Position"`, `"Transform"`, etc.). |
|| `shape_visible` | no       | `true`           | For Controls: shape visibility.                        |
|| `shape_color`   | no       | red              | For Controls: RGBA shape color.                        |

## Output

|| Field          | Type    | Description                           |
|| -------------- | ------- | ------------------------------------- |
|| `element_name` | string  | Name of the created element           |
|| `element_type` | string  | Type of the created element           |
|| `asset_path`   | string  | Control Rig asset path                |
|| `success`      | boolean | Whether creation succeeded            |

## Examples

### Add a bone

```json
{
  "asset_path": "/Game/Rigs/CR_MyRig.CR_MyRig",
  "element_name": "CustomBone",
  "element_type": "Bone"
}
```

### Add a control with parent

```json
{
  "asset_path": "/Game/Rigs/CR_MyRig.CR_MyRig",
  "element_name": "IK_Foot_L",
  "element_type": "Control",
  "parent_name": "Foot_L",
  "parent_type": "Bone",
  "control_type": "Position"
}
```

## Errors

|| Condition                | `isError` | Message                                  |
|| ------------------------ | --------- | ---------------------------------------- |
|| Missing `asset_path`     | true      | 'asset_path' is required                 |
|| Missing `element_name`   | true      | 'element_name' is required               |
|| Missing `element_type`   | true      | 'element_type' is required               |
|| Invalid element type     | true      | Invalid element_type: ...                |
|| Invalid control type     | true      | Invalid control_type: ...                |
|| Parent not found         | true      | Parent element not found: ...            |
|| Asset not found          | true      | Control Rig Blueprint not found or wrong type: ... |
|| Creation failed          | true      | Failed to add ... element '...'          |
