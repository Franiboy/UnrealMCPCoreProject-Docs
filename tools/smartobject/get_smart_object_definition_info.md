# get_smart_object_definition_info

**Category:** Smart Object  
**Source:** `Private/SmartObject/MCPSmartObjectTools_Query.cpp`

## Description

Inspects a Smart Object Definition asset. Returns detailed information about all slots including offset, rotation, activity tags, runtime tags, behavior definitions, and enabled state. Also reports definition-level activity tags, user tag filtering policy, and activity tag merging policy.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Smart Object Definition |

## Response

```json
{
  "name": "SO_Bench",
  "asset_path": "/Game/SmartObjects/SO_Bench.SO_Bench",
  "activity_tags": ["SmartObject.Activity.Sit"],
  "slots": [
    {
      "index": 0,
      "name": "Seat1",
      "id": "A1B2C3D4",
      "offset": { "x": 100.0, "y": 0.0, "z": 0.0 },
      "rotation": { "pitch": 0.0, "yaw": 0.0, "roll": 0.0 },
      "enabled": true,
      "activity_tags": ["SmartObject.Activity.Sit"],
      "runtime_tags": [],
      "behavior_definitions": ["SmartObjectBehaviorDefinition"]
    }
  ],
  "slot_count": 1,
  "user_tags_filtering_policy": "NoFilter",
  "activity_tags_merging_policy": "Combine"
}
```

### Slot Fields

| Field | Type | Description |
|-------|------|-------------|
| `index` | integer | 0-based slot index |
| `name` | string | Slot display name (editor-only) |
| `id` | string | Slot GUID (editor-only) |
| `offset` | object | Position offset (`x`, `y`, `z`) |
| `rotation` | object | Rotation (`pitch`, `yaw`, `roll`) |
| `enabled` | boolean | Whether the slot is enabled |
| `activity_tags` | array | Activity gameplay tags |
| `runtime_tags` | array | Initial runtime gameplay tags |
| `behavior_definitions` | array | Behavior definition class names |

## Examples

### Inspect a definition
```json
{ "tool": "get_smart_object_definition_info", "arguments": { "asset_path": "/Game/SO_Bench" } }
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `asset_path` | `"asset_path is required"` |
| Asset not found | `"Smart Object Definition not found: <path>"` |
