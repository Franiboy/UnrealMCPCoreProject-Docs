# get_hlod_info

Get HLOD (Hierarchical Level of Detail) settings: default HLOD layer, and all HLOD layer assets with their configuration.

## Input

```json
{}
```

No parameters required.

## Output

|| Field                              | Type    | Description                                  |
|| ---------------------------------- | ------- | -------------------------------------------- |
|| `world_partition_enabled`          | boolean | Whether World Partition is enabled            |
|| `default_hlod_layer`               | string  | Path to the default HLOD layer (if WP active) |
|| `hlod_layer_assets`                | array   | Array of HLOD layer asset objects             |
|| `hlod_layer_assets[].name`         | string  | Asset name                                   |
|| `hlod_layer_assets[].asset_path`   | string  | Content path to the HLOD layer asset         |
|| `hlod_layer_assets[].layer_type`   | string  | HLOD layer type name                         |

## Examples

### Basic query

```json
{}
```

## Errors

|| Condition          | `isError` | Message                  |
|| ------------------ | --------- | ------------------------ |
|| No editor world    | true      | No editor world available |
