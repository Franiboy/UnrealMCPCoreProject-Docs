# create_data_layer

Create a new Data Layer. Creates a UDataLayerAsset and registers it with the world's data layer system.

## Input

```json
{
  "name": "DL_Gameplay",
  "path": "/Game/DataLayers",
  "type": "Runtime",
  "initial_runtime_state": "Activated"
}
```

|| Parameter              | Required | Default       | Description                                               |
|| ---------------------- | -------- | ------------- | --------------------------------------------------------- |
|| `name`                 | **yes**  | ---           | Name for the Data Layer asset.                            |
|| `path`                 | no       | `"/Game"`     | Content path for the asset.                               |
|| `type`                 | no       | `"Runtime"`   | Data layer type: `"Runtime"` or `"Editor"`.               |
|| `initial_runtime_state`| no       | `"Unloaded"`  | Initial state: `"Unloaded"`, `"Loaded"`, or `"Activated"`. |

## Output

|| Field         | Type   | Description                           |
|| ------------- | ------ | ------------------------------------- |
|| `name`        | string | Name of the created data layer        |
|| `asset_path`  | string | Content path to the asset             |
|| `type`        | string | Data layer type                       |

## Examples

### Create a runtime data layer

```json
{
  "name": "DL_Gameplay",
  "type": "Runtime"
}
```

### Create an editor-only data layer

```json
{
  "name": "DL_Debug",
  "type": "Editor",
  "path": "/Game/Debug"
}
```

## Errors

|| Condition                  | `isError` | Message                                  |
|| -------------------------- | --------- | ---------------------------------------- |
|| Missing `name`             | true      | 'name' is required                       |
|| Asset already exists       | true      | Data Layer asset already exists: ...     |
|| No editor world            | true      | No editor world available                |
