# get_control_rig_info

Inspect a Control Rig Blueprint: hierarchy elements (bones, controls, nulls, curves, connectors), their transforms, and control settings.

## Input

```json
{
  "asset_path": "/Game/Rigs/CR_Mannequin.CR_Mannequin",
  "element_type_filter": "Control"
}
```

|| Parameter             | Required | Default | Description                                               |
|| --------------------- | -------- | ------- | --------------------------------------------------------- |
|| `asset_path`          | **yes**  | ---     | Content path to the Control Rig Blueprint asset.          |
|| `element_type_filter` | no       | `"All"` | Filter by type: `"Bone"`, `"Control"`, `"Null"`, `"Curve"`, `"Connector"`, or `"All"`. |

## Output

|| Field                               | Type    | Description                              |
|| ----------------------------------- | ------- | ---------------------------------------- |
|| `name`                              | string  | Asset name                               |
|| `asset_path`                        | string  | Full content path                        |
|| `element_count`                     | number  | Number of elements returned              |
|| `elements`                          | array   | Array of hierarchy element objects       |
|| `elements[].name`                   | string  | Element name                             |
|| `elements[].type`                   | string  | Element type (`"Bone"`, `"Control"`, etc.) |
|| `elements[].transform`              | object  | Local transform with location, rotation, scale |
|| `elements[].parent`                 | object  | Parent element name and type (if any)    |
|| `elements[].bone_type`              | string  | For bones: `"Imported"` or `"User"`      |
|| `elements[].control_type`           | string  | For controls: `"Float"`, `"Position"`, `"Transform"`, etc. |
|| `elements[].shape_visible`          | boolean | For controls: shape visibility           |
|| `elements[].shape_color`            | object  | For controls: RGBA shape color           |
|| `elements[].animation_type`         | string  | For controls: animation type             |

## Examples

### Get all elements

```json
{
  "asset_path": "/Game/Rigs/CR_Mannequin.CR_Mannequin"
}
```

### Get only controls

```json
{
  "asset_path": "/Game/Rigs/CR_Mannequin.CR_Mannequin",
  "element_type_filter": "Control"
}
```

## Errors

|| Condition               | `isError` | Message                                    |
|| ----------------------- | --------- | ------------------------------------------ |
|| Missing `asset_path`    | true      | 'asset_path' is required                   |
|| Asset not found         | true      | Control Rig Blueprint not found or wrong type: ... |
|| Invalid type filter     | true      | Invalid element_type_filter: ...           |
