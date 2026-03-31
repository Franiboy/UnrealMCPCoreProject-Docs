# list_attribute_sets

**Category:** Gameplay Ability System (GAS)  
**Source:** `Private/GAS/MCPGASTools_AttributeSet.cpp`

## Description

Lists Attribute Set classes available in the project, including both C++ and Blueprint-defined sets. Scans all loaded `UAttributeSet` subclasses and returns summary info including name, origin, parent class, and attribute count.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `name_filter` | string | No | | Case-insensitive substring match on class name |
| `include_engine` | boolean | No | `false` | Include engine/plugin Attribute Sets |

## Response

```json
{
  "count": 2,
  "attribute_sets": [
    {
      "name": "AS_PlayerStats",
      "class_path": "/Game/AS_PlayerStats.AS_PlayerStats_C",
      "origin": "Blueprint",
      "parent_class": "AttributeSet",
      "attribute_count": 4
    },
    {
      "name": "UMyAttributeSet",
      "class_path": "/Script/MyGame.MyAttributeSet",
      "origin": "C++",
      "parent_class": "AttributeSet",
      "attribute_count": 3
    }
  ]
}
```

## Examples

### List all attribute sets
```json
{ "tool": "list_attribute_sets", "arguments": {} }
```

### Filter by name
```json
{ "tool": "list_attribute_sets", "arguments": { "name_filter": "Player" } }
```

### Include engine sets
```json
{ "tool": "list_attribute_sets", "arguments": { "include_engine": true } }
```

## Error Cases

No errors — returns `count: 0` with empty array if no attribute sets found.
