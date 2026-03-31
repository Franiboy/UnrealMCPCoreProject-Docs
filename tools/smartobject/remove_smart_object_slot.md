# remove_smart_object_slot

**Category:** Smart Object  
**Source:** `Private/SmartObject/MCPSmartObjectTools_Edit.cpp`

## Description

Removes a slot from a Smart Object Definition by its 0-based index. Remaining slots are re-indexed after removal.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Smart Object Definition |
| `slot_index` | integer | **Yes** | | 0-based index of the slot to remove |

## Response

```json
{
  "success": true,
  "removed_index": 1,
  "removed_name": "Seat2",
  "remaining_slots": 2
}
```

> `removed_name` is only included when the slot had a non-empty name.

## Examples

### Remove the first slot
```json
{ "tool": "remove_smart_object_slot", "arguments": { "asset_path": "/Game/SO_Bench", "slot_index": 0 } }
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `asset_path` | `"asset_path is required"` |
| Missing `slot_index` | `"slot_index is required"` |
| Asset not found | `"Smart Object Definition not found: <path>"` |
| Index out of bounds | `"Invalid slot_index: <n> (definition has <m> slots)"` |
| Internal reflection failure | `"Failed to find Slots property on Smart Object Definition"` |
