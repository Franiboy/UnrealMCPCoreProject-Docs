# set_world_partition_settings

Configure World Partition settings: streaming, and default HLOD layer.

## Input

```json
{
  "enable_streaming": true,
  "default_hlod_layer": "/Game/HLOD/DefaultHLODLayer.DefaultHLODLayer"
}
```

|| Parameter            | Required | Default | Description                                  |
|| -------------------- | -------- | ------- | -------------------------------------------- |
|| `enable_streaming`   | no       | ---     | Enable or disable streaming.                 |
|| `default_hlod_layer` | no       | ---     | Asset path of the default HLOD layer.        |

At least one setting must be provided.

## Output

|| Field            | Type   | Description                           |
|| ---------------- | ------ | ------------------------------------- |
|| `properties_set` | number | Number of settings changed            |
|| `values`         | object | Key-value pairs of settings applied   |

## Examples

### Enable streaming

```json
{
  "enable_streaming": true
}
```

### Set HLOD layer

```json
{
  "default_hlod_layer": "/Game/HLOD/MyHLODLayer.MyHLODLayer"
}
```

## Errors

|| Condition              | `isError` | Message                                              |
|| ---------------------- | --------- | ---------------------------------------------------- |
|| No settings provided   | true      | No settings provided. Use 'enable_streaming' or 'default_hlod_layer'. |
|| HLOD layer not found   | true      | HLOD Layer not found: ...                            |
|| WP not enabled         | true      | World Partition is not enabled for the current world |
|| No editor world        | true      | No editor world available                            |
