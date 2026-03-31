# create_attribute_set

**Category:** Gameplay Ability System (GAS)  
**Source:** `Private/GAS/MCPGASTools_AttributeSet.cpp`

## Description

Creates a new Attribute Set Blueprint asset, optionally with initial `FGameplayAttributeData` variables. The Blueprint is compiled and saved immediately. A custom parent class can be specified (must derive from `UAttributeSet`).

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `name` | string | **Yes** | | Name of the Attribute Set Blueprint |
| `path` | string | No | `/Game` | Content path for the asset |
| `parent_class` | string | No | `UAttributeSet` | Parent class path (native or Blueprint). Must derive from `UAttributeSet` |
| `attributes` | string[] | No | `[]` | Array of attribute names to add as `FGameplayAttributeData` variables (e.g. `["Health", "Mana"]`) |

## Response

```json
{
  "name": "AS_PlayerStats",
  "asset_path": "/Game/AS_PlayerStats.AS_PlayerStats",
  "parent_class": "AttributeSet",
  "attributes_added": ["Health", "Mana", "Stamina"],
  "attribute_count": 3
}
```

## Examples

### Create with initial attributes
```json
{
  "tool": "create_attribute_set",
  "arguments": {
    "name": "AS_PlayerStats",
    "path": "/Game/Attributes",
    "attributes": ["Health", "Mana", "Stamina", "Armor"]
  }
}
```

### Create empty (add attributes later)
```json
{
  "tool": "create_attribute_set",
  "arguments": { "name": "AS_EnemyStats" }
}
```

### Create with custom parent class
```json
{
  "tool": "create_attribute_set",
  "arguments": {
    "name": "AS_SpecialStats",
    "parent_class": "/Game/AS_BaseStats.AS_BaseStats_C"
  }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `name` | `name is required` |
| Asset already exists | `Asset already exists: <path>` |
| Invalid parent class | `parent_class '<path>' is not derived from UAttributeSet` |
| Parent class not found | `parent_class not found or not a valid AttributeSet subclass: <path>` |
