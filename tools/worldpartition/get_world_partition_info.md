# get_world_partition_info

Inspect World Partition settings: enabled state, streaming configuration, grid info, HLOD defaults, and data layer summary.

## Input

```json
{}
```

No parameters required.

## Output

|| Field                        | Type    | Description                                      |
|| ---------------------------- | ------- | ------------------------------------------------ |
|| `world_partition_enabled`    | boolean | Whether World Partition is enabled                |
|| `streaming_enabled`          | boolean | Whether streaming is enabled (if WP active)       |
|| `data_layer_count`           | number  | Number of data layers in the world                |
|| `default_hlod_layer`         | string  | Path to the default HLOD layer (if set)           |
|| `world_name`                 | string  | Name of the current editor world                  |

When World Partition is disabled, returns `world_partition_enabled: false` with basic world info only.

## Examples

### Basic query

```json
{}
```

## Errors

|| Condition            | `isError` | Message                  |
|| -------------------- | --------- | ------------------------ |
|| No editor world      | true      | No editor world available |
