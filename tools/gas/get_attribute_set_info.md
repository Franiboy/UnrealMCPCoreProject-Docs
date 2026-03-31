# get_attribute_set_info

**Category:** Gameplay Ability System (GAS)  
**Source:** `Private/GAS/MCPGASTools_AttributeSet.cpp`

## Description

Inspects an Attribute Set class and returns detailed information about all its `FGameplayAttributeData` attributes. Works with both C++ and Blueprint Attribute Sets. Reads default values from the CDO (Class Default Object).

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `class_name` | string | **Yes** | | Class name or full path of the Attribute Set. Accepts short name (`MyAttributeSet`), prefixed (`UMyAttributeSet`), or a Blueprint asset path (`/Game/AS_Player`) |

## Response

```json
{
  "name": "AS_PlayerStats_C",
  "class_path": "/Game/AS_PlayerStats.AS_PlayerStats_C",
  "origin": "Blueprint",
  "parent_class": "AttributeSet",
  "blueprint_path": "/Game/AS_PlayerStats.AS_PlayerStats",
  "attribute_count": 3,
  "attributes": [
    {
      "name": "Health",
      "base_value": 0,
      "current_value": 0
    },
    {
      "name": "Mana",
      "base_value": 0,
      "current_value": 0
    },
    {
      "name": "Stamina",
      "base_value": 0,
      "current_value": 0
    }
  ]
}
```

## Examples

### Inspect by Blueprint asset path
```json
{ "tool": "get_attribute_set_info", "arguments": { "class_name": "/Game/AS_PlayerStats" } }
```

### Inspect by short class name
```json
{ "tool": "get_attribute_set_info", "arguments": { "class_name": "MyAttributeSet" } }
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `class_name` | `class_name is required` |
| Class not found | `Attribute Set class not found: <name>` |
| Path not an Attribute Set | `Attribute Set not found at path: <path>` |
