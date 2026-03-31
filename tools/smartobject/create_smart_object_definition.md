# create_smart_object_definition

**Category:** Smart Object  
**Source:** `Private/SmartObject/MCPSmartObjectTools_Edit.cpp`

## Description

Creates a new Smart Object Definition asset (`USmartObjectDefinition`). The new definition starts with zero slots. Optionally sets definition-level activity tags.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `name` | string | **Yes** | | Name of the Smart Object Definition asset |
| `path` | string | No | `/Game` | Content path for the new asset |
| `activity_tags` | string[] | No | | Activity gameplay tags to set on the definition |

## Response

```json
{
  "name": "SO_Bench",
  "asset_path": "/Game/SO_Bench.SO_Bench",
  "slot_count": 0
}
```

## Examples

### Create a basic definition
```json
{ "tool": "create_smart_object_definition", "arguments": { "name": "SO_Bench" } }
```

### Create in a subfolder with tags
```json
{
  "tool": "create_smart_object_definition",
  "arguments": {
    "name": "SO_Workstation",
    "path": "/Game/SmartObjects",
    "activity_tags": ["SmartObject.Activity.Work"]
  }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `name` | `"name is required"` |
| Asset already exists | `"Smart Object Definition already exists: <path>"` |
| Package creation failure | `"Failed to create package: <path>"` |
