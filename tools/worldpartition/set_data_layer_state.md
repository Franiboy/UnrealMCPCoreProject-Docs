# set_data_layer_state

Set Data Layer state: editor visibility, loaded-in-editor state, and initial runtime state.

## Input

```json
{
  "data_layer_name": "DL_Gameplay",
  "visible": true,
  "loaded_in_editor": true,
  "initial_runtime_state": "Activated"
}
```

|| Parameter              | Required | Default | Description                                              |
|| ---------------------- | -------- | ------- | -------------------------------------------------------- |
|| `data_layer_name`      | **yes**  | ---     | Name of the Data Layer to modify.                        |
|| `visible`              | no       | ---     | Set editor visibility.                                   |
|| `loaded_in_editor`     | no       | ---     | Set loaded-in-editor state.                              |
|| `initial_runtime_state`| no       | ---     | Set initial runtime state: `"Unloaded"`, `"Loaded"`, or `"Activated"`. |

At least one state property must be provided.

## Output

|| Field                     | Type    | Description                        |
|| ------------------------- | ------- | ---------------------------------- |
|| `data_layer_name`         | string  | Name of the modified data layer    |
|| `properties_set`          | number  | Number of properties changed       |
|| `values`                  | object  | Key-value pairs of properties set  |

## Examples

### Set visibility

```json
{
  "data_layer_name": "DL_Gameplay",
  "visible": false
}
```

### Set runtime state

```json
{
  "data_layer_name": "DL_Gameplay",
  "initial_runtime_state": "Loaded"
}
```

## Errors

|| Condition                     | `isError` | Message                             |
|| ----------------------------- | --------- | ----------------------------------- |
|| Missing `data_layer_name`     | true      | 'data_layer_name' is required       |
|| Data layer not found          | true      | Data Layer not found: ...           |
|| No state properties provided  | true      | No state properties provided        |
|| No editor world               | true      | No editor world available           |
