# add_comment

Add a comment box to a Blueprint graph.

## Category
Blueprint — Graph / Node Tools

## Parameters

| Name           | Type    | Required | Description |
| -------------- | ------- | -------- | ----------- |
| `asset_path`   | string  | Yes      | Blueprint asset path (e.g. `/Game/MyBP`) |
| `comment_text` | string  | Yes      | Comment text to display in the comment box |
| `graph_name`   | string  | No       | Target graph name (default: `EventGraph`) |
| `position_x`   | integer | No       | X position in graph coordinates (default: 0) |
| `position_y`   | integer | No       | Y position in graph coordinates (default: 0) |
| `size_x`       | integer | No       | Width of the comment box (default: 400) |
| `size_y`       | integer | No       | Height of the comment box (default: 200) |
| `font_size`    | integer | No       | Font size of the comment text, 1-1000 (default: 18) |
| `move_mode`    | string  | No       | `group` (default) = moves enclosed nodes; `comment` = floating, does not move enclosed nodes |
| `color`        | object  | No       | Comment color as `{r, g, b, a}` floats 0-1 (default: white) |

## Behaviour

1. Locates the target graph in the Blueprint.
2. Creates a `UEdGraphNode_Comment` node in the graph.
3. Sets position, size, font size, move mode, and color.
4. Marks the Blueprint as modified (no recompilation — cosmetic change).

## Response

```jsonc
{
  "asset_path": "/Game/MyBP",
  "graph_name": "EventGraph",
  "node_id": "EdGraphNode_Comment_0",
  "comment_text": "My Comment",
  "position": { "x": 0, "y": 0 },
  "size": { "x": 400, "y": 200 },
  "font_size": 18,
  "move_mode": "group",
  "color": { "r": 1.0, "g": 1.0, "b": 1.0, "a": 1.0 }
}
```

## Undo

The tool emits a `remove_node` undo action (same as `add_node`). Calling undo removes the comment node from the graph.

## Errors

| Condition | Message (contains) |
| --------- | ------------------ |
| Blueprint not found | `not found` |
| Graph not found | `not found` |
| Missing required parameters | `required` |

## Examples

### Add a basic comment

```json
{
  "tool": "add_comment",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "comment_text": "Event handling logic"
  }
}
```

### Add a positioned, colored comment box

```json
{
  "tool": "add_comment",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "comment_text": "Input processing",
    "position_x": 200,
    "position_y": -100,
    "size_x": 800,
    "size_y": 400,
    "font_size": 24,
    "move_mode": "group",
    "color": { "r": 0.2, "g": 0.8, "b": 0.3, "a": 1.0 }
  }
}
```

### Add a floating (non-grouping) comment

```json
{
  "tool": "add_comment",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "comment_text": "TODO: refactor this",
    "move_mode": "comment"
  }
}
```
