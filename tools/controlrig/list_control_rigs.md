# list_control_rigs

List Control Rig Blueprint assets with optional name/path filtering.

## Input

```json
{
  "name_filter": "Mannequin",
  "path_prefix": "/Game/Rigs",
  "include_engine": false,
  "include_details": false
}
```

|| Parameter         | Required | Default   | Description                                  |
|| ----------------- | -------- | --------- | -------------------------------------------- |
|| `name_filter`     | no       | ---       | Substring match on asset name.               |
|| `path_prefix`     | no       | all paths | Content path prefix to restrict search.      |
|| `include_engine`  | no       | `false`   | Include engine/plugin assets in results.     |
|| `include_details` | no       | `false`   | Load each asset for element counts (`bone_count`, etc.). Slower. |

## Output

|| Field                            | Type   | Description                              |
|| -------------------------------- | ------ | ---------------------------------------- |
|| `count`                          | number | Number of Control Rigs returned          |
|| `control_rigs`                   | array  | Array of Control Rig objects             |
|| `control_rigs[].name`            | string | Asset name                               |
|| `control_rigs[].asset_path`      | string | Content path to the asset                |
|| `control_rigs[].element_count`   | number | Total hierarchy elements *(details only)* |
|| `control_rigs[].bone_count`      | number | Number of bone elements *(details only)* |
|| `control_rigs[].control_count`   | number | Number of control elements *(details only)* |
|| `control_rigs[].null_count`      | number | Number of null/space elements *(details only)* |
|| `control_rigs[].curve_count`     | number | Number of curve elements *(details only)* |

## Examples

### List all game Control Rigs

```json
{}
```

### Filter by name

```json
{
  "name_filter": "IK"
}
```

### List with element counts

```json
{
  "name_filter": "IK",
  "include_details": true
}
```

## Errors

|| Condition | `isError` | Message |
|| --------- | --------- | ------- |
|| *(none)*  | ---       | Always succeeds; returns empty array if no matches. |
