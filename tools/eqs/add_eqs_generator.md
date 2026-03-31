# add_eqs_generator

**Category:** EQS (Environment Query System)  
**Source:** `Private/EQS/MCPEQSTools_Edit.cpp`

## Description

Adds a generator (wrapped in its own option) to an EQS Query. Generators create items (points or actors) that tests evaluate. Each option in an EQS Query has exactly one generator and zero or more tests.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the EQS Query |
| `generator_class` | string | **Yes** | | Generator class name. Prefix `EnvQueryGenerator_` is optional. |
| `option_name` | string | No | auto | Display name for the option |
| `properties` | object | No | | Key-value pairs to set on the generator via reflection |

### Built-in Generator Classes

| Short Name | Full Class | Item Type |
|-----------|-----------|-----------|
| `SimpleGrid` | `EnvQueryGenerator_SimpleGrid` | Point |
| `OnCircle` | `EnvQueryGenerator_OnCircle` | Point |
| `Donut` | `EnvQueryGenerator_Donut` | Point |
| `Cone` | `EnvQueryGenerator_Cone` | Point |
| `ActorsOfClass` | `EnvQueryGenerator_ActorsOfClass` | Actor |
| `CurrentLocation` | `EnvQueryGenerator_CurrentLocation` | Point |
| `PathingGrid` | `EnvQueryGenerator_PathingGrid` | Point |
| `Composite` | `EnvQueryGenerator_Composite` | Varies |
| `PerceivedActors` | `EnvQueryGenerator_PerceivedActors` | Actor |

## Response

```json
{
  "option_index": 0,
  "generator_class": "EnvQueryGenerator_SimpleGrid",
  "item_type": "EnvQueryItemType_Point",
  "total_options": 1,
  "properties_applied": ["GridSize"],
  "properties_failed": []
}
```

## Examples

### Add a SimpleGrid generator
```json
{ "tool": "add_eqs_generator", "arguments": { "asset_path": "/Game/EQ_FindCover", "generator_class": "SimpleGrid" } }
```

### Add an OnCircle generator with custom name
```json
{ "tool": "add_eqs_generator", "arguments": { "asset_path": "/Game/EQ_FindCover", "generator_class": "OnCircle", "option_name": "Circle Points" } }
```

## Error Cases

- `asset_path is required` — parameter missing or empty
- `generator_class is required` — parameter missing or empty
- `Generator class not found: <name>` — no matching generator class
- `EQS Query not found: <path>` — no query at that path
