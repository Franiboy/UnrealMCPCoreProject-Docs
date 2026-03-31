# set_pin_default

Set the default value on an input pin of a Blueprint graph node.

## Category
Blueprint — Graph / Node Tools

## Parameters

| Name             | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| `asset_path`     | string | Yes      | Blueprint asset path (e.g. `/Game/MyBP`) |
| `node_id`        | string | Yes      | Node ID (e.g. `K2Node_CallFunction_0`). Use `get_blueprint_graph` to discover node IDs. |
| `pin_id`         | string | Yes      | Pin name (e.g. `InString`, `Duration`). Must be an unconnected input pin. |
| `default_value`  | string | No*      | New default value as a string. For booleans use `true`/`false`, for vectors use `(X=0,Y=0,Z=0)`, for enums use the enum value name. |
| `default_object` | string | No*      | Object reference path for object/class pins (e.g. `/Game/MyAsset.MyAsset`). Pass empty string to clear. |
| `graph_name`     | string | No       | Optional graph name to narrow the node search. If omitted, all graphs are searched. |

\* At least one of `default_value` or `default_object` must be provided.

## Behaviour

1. Locates the specified node and pin in the Blueprint's graphs.
2. Validates the pin is an **input** pin and is **not connected** to another pin.
3. Captures the current default value for the undo payload.
4. Sets the new default value via the K2 schema and direct assignment.
5. Notifies the owning node of the change (`PinDefaultValueChanged`).
6. Compiles the Blueprint and marks the package dirty.

## Response

```jsonc
{
  "asset_path": "/Game/MyBP",
  "graph_name": "EventGraph",
  "node_id": "K2Node_CallFunction_0",
  "pin_id": "InString",
  "direction": "input",
  "type": "FString",
  "default_value": "Hello MCP",
  // only present for object reference pins:
  "default_object": "/Game/MyAsset.MyAsset",
  "previous": {
    "default_value": "Hello",
    "default_object": ""        // only if it had one
  }
}
```

## Undo

The tool emits `restore_pin_default` undo data containing the old `default_value` and `default_object`. Calling undo restores the previous value and recompiles.

## Errors

| Condition | Message (contains) |
| --------- | ------------------ |
| Blueprint not found | `not found` |
| Node not found | `not found` |
| Pin not found | `not found` |
| Pin is an output | `output pin` |
| Pin is connected | `connected` |
| Neither `default_value` nor `default_object` provided | `default_value` |
| Object path not loadable | `not found` |

## Examples

### Set a string on PrintString

```json
{
  "tool": "set_pin_default",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "node_id": "K2Node_CallFunction_0",
    "pin_id": "InString",
    "default_value": "Hello World"
  }
}
```

### Set a float on Delay

```json
{
  "tool": "set_pin_default",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "node_id": "K2Node_CallFunction_1",
    "pin_id": "Duration",
    "default_value": "5.0"
  }
}
```

### Set a boolean

```json
{
  "tool": "set_pin_default",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "node_id": "K2Node_CallFunction_0",
    "pin_id": "bPrintToScreen",
    "default_value": "false"
  }
}
```
