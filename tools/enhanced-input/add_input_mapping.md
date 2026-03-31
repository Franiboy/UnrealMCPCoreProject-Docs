# add_input_mapping

**Category:** Enhanced Input  
**Source:** `Private/EnhancedInput/MCPEnhancedInputTools_Edit.cpp`

## Description

Adds an action mapping to an Input Mapping Context. Binds an Input Action to a key with optional triggers and modifiers.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `context_path` | string | **Yes** | | Asset path of the Input Mapping Context |
| `action_path` | string | **Yes** | | Asset path of the Input Action to map |
| `key` | string | **Yes** | | Key name to bind (e.g. `SpaceBar`, `W`, `Gamepad_FaceButton_Bottom`, `MouseX`) |
| `triggers` | string[] | No | | Trigger class names (e.g. `InputTriggerPressed`, `InputTriggerDown`, `InputTriggerHold`, `InputTriggerTap`) |
| `modifiers` | string[] | No | | Modifier class names (e.g. `InputModifierNegate`, `InputModifierSwizzleAxis`, `InputModifierDeadZone`, `InputModifierScalar`) |

## Response

```json
{
  "context": "IMC_Default",
  "context_path": "/Game/IMC_Default.IMC_Default",
  "action": "IA_Jump",
  "key": "SpaceBar",
  "mapping_index": 0,
  "num_mappings": 1
}
```

## Examples

### Bind Jump to SpaceBar
```json
{
  "tool": "add_input_mapping",
  "arguments": {
    "context_path": "/Game/IMC_Default",
    "action_path": "/Game/IA_Jump",
    "key": "SpaceBar"
  }
}
```

### Bind Move with trigger and modifier
```json
{
  "tool": "add_input_mapping",
  "arguments": {
    "context_path": "/Game/IMC_Default",
    "action_path": "/Game/IA_Move",
    "key": "S",
    "triggers": ["InputTriggerDown"],
    "modifiers": ["InputModifierNegate"]
  }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing required params | `"context_path, action_path, and key are required"` |
| Context not found | `"Input Mapping Context not found: <path>"` |
| Action not found | `"Input Action not found: <path>"` |
| Invalid key | `"Invalid key name: <key>"` |
| Unknown trigger class | `"Unknown trigger class: <name>"` |
| Unknown modifier class | `"Unknown modifier class: <name>"` |
