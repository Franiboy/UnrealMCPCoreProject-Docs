# list_data_layers

List all Data Layers in the current world with name, type, initial runtime state, visibility, and hierarchy.

## Input

```json
{
  "name_filter": "Gameplay",
  "type_filter": "Runtime"
}
```

|| Parameter     | Required | Default | Description                                      |
|| ------------- | -------- | ------- | ------------------------------------------------ |
|| `name_filter` | no       | ---     | Substring match on data layer name.              |
|| `type_filter` | no       | ---     | Filter by type: `"Runtime"` or `"Editor"`.       |

## Output

|| Field                              | Type    | Description                           |
|| ---------------------------------- | ------- | ------------------------------------- |
|| `count`                            | number  | Number of data layers returned        |
|| `data_layers`                      | array   | Array of data layer objects           |
|| `data_layers[].name`               | string  | Data layer short name                 |
|| `data_layers[].type`               | string  | `"Runtime"` or `"Editor"`             |
|| `data_layers[].initial_runtime_state` | string | `"Unloaded"`, `"Loaded"`, or `"Activated"` |
|| `data_layers[].is_visible`         | boolean | Editor visibility state               |
|| `data_layers[].is_loaded_in_editor`| boolean | Whether loaded in editor              |

## Examples

### List all data layers

```json
{}
```

### Filter by name

```json
{
  "name_filter": "Gameplay"
}
```

### Filter by type

```json
{
  "type_filter": "Runtime"
}
```

## Errors

|| Condition          | `isError` | Message                         |
|| ------------------ | --------- | ------------------------------- |
|| No editor world    | true      | No editor world available       |
|| *(none otherwise)* | ---       | Always succeeds; returns empty array if no matches. |
