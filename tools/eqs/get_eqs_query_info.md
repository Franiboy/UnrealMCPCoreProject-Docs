# get_eqs_query_info

**Category:** EQS (Environment Query System)  
**Source:** `Private/EQS/MCPEQSTools_Query.cpp`

## Description

Inspects an EQS Query template asset in detail. Returns the full option hierarchy including generator class, item type, auto-sort setting, and all tests with their purpose, filter type, scoring equation, and scoring factor.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the EQS Query |

## Response

```json
{
  "name": "EQ_FindCover",
  "asset_path": "/Game/AI/EQ_FindCover.EQ_FindCover",
  "option_count": 1,
  "options": [
    {
      "index": 0,
      "generator": {
        "class": "EnvQueryGenerator_SimpleGrid",
        "option_name": "Grid Points",
        "item_type": "EnvQueryItemType_Point",
        "auto_sort_tests": true
      },
      "tests": [
        {
          "index": 0,
          "class": "EnvQueryTest_Distance",
          "purpose": "Score",
          "test_order": 0,
          "scoring_equation": "InverseLinear",
          "scoring_factor": 1.0
        }
      ],
      "test_count": 1
    }
  ]
}
```

## Examples

### Inspect a query
```json
{ "tool": "get_eqs_query_info", "arguments": { "asset_path": "/Game/AI/EQ_FindCover" } }
```

## Error Cases

- `asset_path is required` — parameter missing or empty
- `EQS Query not found: <path>` — no query at that path
