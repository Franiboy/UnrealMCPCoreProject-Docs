# get_blueprint_node

Get full detail for a single node in a Blueprint graph. Returns all pins (including hidden), node metadata, breakpoint status, and a reflection-based `properties` bag containing every UPROPERTY on the node's actual class. Use `get_blueprint_graph` first to discover `node_id`s, then drill into a specific node with this tool.

## Input

```json
{
  "asset_path": "/Game/Blueprints/BP_MyActor",
  "node_id": "K2Node_Event_0",
  "graph_name": "EventGraph",
  "include_hidden_pins": false
}
```

| Parameter             | Required | Description                                                       |
| --------------------- | -------- | ----------------------------------------------------------------- |
| `asset_path`          | yes      | Asset path of the Blueprint                                       |
| `node_id`             | yes      | Node name from `get_blueprint_graph` output (e.g. `K2Node_Event_0`) |
| `graph_name`          | no       | Narrow search to a specific graph (omit to search all graphs)     |
| `include_hidden_pins` | no       | Include hidden/internal pins (default: `false`)                   |

## Output

```json
{
  "node_id": "K2Node_Event_0",
  "node_class": "K2Node_Event",
  "class_hierarchy": ["K2Node_Event", "K2Node", "EdGraphNode"],
  "title": "Event BeginPlay",
  "full_title": "Event BeginPlay",
  "position": { "x": -200, "y": 0 },
  "is_deprecated": false,
  "has_breakpoint": false,
  "tooltip": "Event when play begins for this actor.",
  "pin_count": 2,
  "hidden_pin_count": 1,
  "pins": [
    {
      "pin_id": "then",
      "direction": "output",
      "type": "exec",
      "is_hidden": false,
      "connected_to": [
        { "node_id": "K2Node_CallFunction_0", "pin_id": "execute" }
      ]
    }
  ],
  "properties": {
    "NodeGuid": "A1B2C3D4...",
    "EnabledState": "Enabled",
    "bHasCompilerMessage": false,
    "FunctionReference": "(MemberName=\"ReceiveBeginPlay\")"
  },
  "asset_path": "/Game/Blueprints/BP_MyActor",
  "blueprint_name": "BP_MyActor",
  "graph_name": "EventGraph",
  "graph_type": "UbergraphPage"
}
```

## Core Fields

| Field                 | Type   | Description                                                       |
| --------------------- | ------ | ----------------------------------------------------------------- |
| `node_id`             | string | Unique node identifier within the graph                           |
| `node_class`          | string | Node class (e.g. `K2Node_Event`, `K2Node_CallFunction`)          |
| `class_hierarchy`     | array  | Full class chain from specific to base (e.g. `K2Node_Event` > `K2Node` > `EdGraphNode`) |
| `title`               | string | Human-readable title                                              |
| `full_title`          | string | Extended title (only if different from `title`)                   |
| `editable_title`      | string | Editable portion of the title (only if different from `title`)    |
| `position`            | object | `{ "x": int, "y": int }` position in graph editor               |
| `comment`             | string | Node comment text (if any)                                        |
| `is_deprecated`       | bool   | Whether the node is deprecated                                    |
| `deprecation_message` | string | Why deprecated (only if `is_deprecated`)                          |
| `tooltip`             | string | Node tooltip text                                                 |
| `has_breakpoint`      | bool   | Whether a breakpoint is set on this node                          |
| `breakpoint_enabled`  | bool   | Whether the breakpoint is active (only if `has_breakpoint`)       |

## Context Fields

| Field              | Type   | Description                                                       |
| ------------------ | ------ | ----------------------------------------------------------------- |
| `asset_path`       | string | Blueprint asset path                                              |
| `blueprint_name`   | string | Blueprint name                                                    |
| `graph_name`       | string | Name of the graph containing this node                            |
| `graph_type`       | string | `UbergraphPage`, `FunctionGraph`, `MacroGraph`, `DelegateSignatureGraph` |

## Pin Fields (Detailed)

| Field                         | Type   | Description                                             |
| ----------------------------- | ------ | ------------------------------------------------------- |
| `pin_id`                      | string | Pin name (unique within a node)                         |
| `direction`                   | string | `input` or `output`                                     |
| `type`                        | string | Pin type (e.g. `exec`, `bool`, `float`)                |
| `friendly_name`               | string | Display name if different from `pin_id`                 |
| `is_hidden`                   | bool   | Hidden internal pin                                     |
| `is_orphaned`                 | bool   | Orphaned pin (left over from recompile)                 |
| `is_reference`                | bool   | Pass-by-reference pin                                   |
| `is_const`                    | bool   | Const pin                                               |
| `sub_category_object`         | string | Referenced type path (for struct/object/enum pins)      |
| `sub_category`                | string | Pin sub-category                                        |
| `default_value`               | string | Default value text                                      |
| `default_object`              | string | Default object path (for object-type pins)              |
| `default_text_value`          | string | Default FText value                                     |
| `autogenerated_default_value` | string | Engine-autogenerated default                            |
| `connected_to[]`              | array  | Array of `{ "node_id", "pin_id" }` connection targets  |

## Reflection Properties Bag

The `properties` object contains **every UPROPERTY** on the node's actual C++ class, exported via Unreal's reflection system. This automatically includes subclass-specific fields without requiring per-type code:

| Node Class             | Example Properties Included                                    |
| ---------------------- | -------------------------------------------------------------- |
| Any `UEdGraphNode`     | `NodeGuid`, `EnabledState`, `AdvancedPinDisplay`, `bHasCompilerMessage`, `ErrorType`, `ErrorMsg`, `bCanRenameNode`, `bCommentBubblePinned` |
| `K2Node_CallFunction`  | `FunctionReference` (member name + class), `bIsPureFunc`, `bIsConstFunc` |
| `K2Node_Event`         | `EventReference`, `FunctionReference`                          |
| `K2Node_VariableGet`   | `VariableReference` (variable name + scope)                    |
| `K2Node_VariableSet`   | `VariableReference`                                            |
| `K2Node_DynamicCast`   | `TargetType`                                                   |
| `K2Node_MacroInstance`  | `MacroGraphReference`                                         |

Booleans are native JSON `true`/`false`, numbers are native JSON numbers, enums are exported as their name string (e.g. `"Enabled"` not `0`). Structs and objects use Unreal's `ExportText` format.
