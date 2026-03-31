# add_smart_object_slot

**Category:** Smart Object  
**Source:** `Private/SmartObject/MCPSmartObjectTools_Edit.cpp`

## Description

Adds a new slot to a Smart Object Definition. Each slot represents an interaction point (e.g. a seat on a bench). Configure the slot's position offset, rotation, display name, enabled state, activity tags, and runtime tags.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Smart Object Definition |
| `slot_name` | string | No | | Display name for the slot |
| `offset` | object | No | `{0,0,0}` | Position offset from parent (`x`, `y`, `z`) |
| `rotation` | object | No | `{0,0,0}` | Rotation relative to parent (`pitch`, `yaw`, `roll`) |
| `enabled` | boolean | No | `true` | Whether the slot is enabled |
| `activity_tags` | string[] | No | | Activity gameplay tags for this slot |
| `runtime_tags` | string[] | No | | Initial runtime gameplay tags for this slot |

## Response

```json
{
  "slot_index": 0,
  "slot_name": "Seat1",
  "slot_id": "A1B2C3D4",
  "enabled": true,
  "total_slots": 1
}
```

## Examples

### Add a basic slot
```json
{ "tool": "add_smart_object_slot", "arguments": { "asset_path": "/Game/SO_Bench" } }
```

### Add a named slot with offset
```json
{
  "tool": "add_smart_object_slot",
  "arguments": {
    "asset_path": "/Game/SO_Bench",
    "slot_name": "LeftSeat",
    "offset": { "x": -100, "y": 0, "z": 0 }
  }
}
```

### Add a disabled slot with rotation and tags
```json
{
  "tool": "add_smart_object_slot",
  "arguments": {
    "asset_path": "/Game/SO_Bench",
    "slot_name": "Reserved",
    "rotation": { "pitch": 0, "yaw": 90, "roll": 0 },
    "enabled": false,
    "activity_tags": ["SmartObject.Activity.Sit"]
  }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `asset_path` | `"asset_path is required"` |
| Asset not found | `"Smart Object Definition not found: <path>"` |
| Internal reflection failure | `"Failed to find Slots property on Smart Object Definition"` |
