# set_pin_default

Set the default value on an input pin of a Blueprint graph node.

## Category
Blueprint - Graph / Node Tools

## Parameters

| Name             | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| `asset_path`     | string | Yes      | Blueprint asset path (e.g. `/Game/MyBP`) |
| `node_id`        | string | Yes      | Node ID (e.g. `K2Node_CallFunction_0`). Use `get_blueprint_graph` to discover node IDs. |
| `pin_id`         | string | Yes      | Pin name (e.g. `InString`, `Duration`). Must be an unconnected input pin. |
| `default_value`  | string | No*      | New default value as a string. For booleans use `true`/`false`, for vectors use `(X=0,Y=0,Z=0)`, for enums use the enum value name. For class/object pins (`TSubclassOf<>`, `UObject*`, etc.), you may pass the asset path here. It will be auto-resolved to `DefaultObject`. |
| `default_object` | string | No*      | Object reference path for object/class pins (e.g. `/Game/MyAsset.MyAsset`, `/Script/Engine.Actor`). Pass empty string to clear. |
| `graph_name`     | string | No       | Optional graph name to narrow the node search. If omitted, all graphs are searched. |

\* At least one of `default_value` or `default_object` must be provided.

## Behaviour

1. Locates the specified node and pin in the Blueprint's graphs.
2. Validates the pin is an **input** pin and is **not connected** to another pin.
3. Captures the current default value for the undo payload.
4. **For class/object pins** (`PC_Class`, `PC_SoftClass`, `PC_Object`, `PC_SoftObject`, `PC_Interface`): if `default_value` is provided instead of `default_object`, the string is automatically resolved to a `UObject*` and stored as `Pin->DefaultObject`. This means you can use `default_value` with a class/asset path and it will just work.
5. Sets the new default value via the K2 schema and direct assignment (for primitive pins) or object resolution (for class/object pins).
6. Notifies the owning node of the change (`PinDefaultValueChanged`).
7. Compiles the Blueprint and marks the package dirty.

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

### Set a class pin (TSubclassOf)

```json
{
  "tool": "set_pin_default",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "node_id": "K2Node_CallFunction_2",
    "pin_id": "ActorClass",
    "default_value": "/Script/Engine.Character"
  }
}
```

The `default_value` is automatically resolved to `DefaultObject` for class/object pins, so you don't need to use `default_object` explicitly (though you still can).
