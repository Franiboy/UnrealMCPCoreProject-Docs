# move_node

Reposition a node in a Blueprint graph editor.

## Category
Blueprint — Graph / Node Tools

## Parameters

| Name         | Type    | Required | Description |
| ------------ | ------- | -------- | ----------- |
| `asset_path` | string  | Yes      | Blueprint asset path (e.g. `/Game/MyBP`) |
| `node_id`    | string  | Yes      | Node ID (e.g. `K2Node_Event_0`). Use `get_blueprint_graph` to discover node IDs. |
| `position_x` | integer | No*      | New X position in graph coordinates. Omit to keep the current X. |
| `position_y` | integer | No*      | New Y position in graph coordinates. Omit to keep the current Y. |
| `graph_name` | string  | No       | Optional graph name to narrow the node search. If omitted, all graphs are searched. |

\* At least one of `position_x` or `position_y` must be provided.

## Behaviour

1. Locates the specified node in the Blueprint's graphs.
2. Captures the current position for the undo payload.
3. Updates `NodePosX` and/or `NodePosY` on the node.
4. Marks the Blueprint as modified (no recompilation — this is a cosmetic change).

## Response

```jsonc
{
  "asset_path": "/Game/MyBP",
  "graph_name": "EventGraph",
  "node_id": "K2Node_CallFunction_0",
  "title": "Print String",
  "position": { "x": 500, "y": 600 },
  "previous_position": { "x": 100, "y": 200 }
}
```

## Undo

The tool emits `restore_node_position` undo data containing the old X and Y. Calling undo restores the previous position.

## Errors

| Condition | Message (contains) |
| --------- | ------------------ |
| Blueprint not found | `not found` |
| Node not found | `not found` |
| Neither `position_x` nor `position_y` provided | `position_x` |
| Missing required parameters | `required` |

## Examples

### Move a node to a specific position

```json
{
  "tool": "move_node",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "node_id": "K2Node_CallFunction_0",
    "position_x": 500,
    "position_y": 600
  }
}
```

### Move only one axis

```json
{
  "tool": "move_node",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "node_id": "K2Node_Event_0",
    "position_x": 0
  }
}
```
