# add_eqs_test

**Category:** EQS (Environment Query System)  
**Source:** `Private/EQS/MCPEQSTools_Edit.cpp`

## Description

Adds a test to a generator/option in an EQS Query. Tests filter and/or score the items produced by a generator. Each test has a purpose (Filter, Score, or FilterAndScore) and can have additional properties set via reflection.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the EQS Query |
| `option_index` | integer | **Yes** | | 0-based index of the option to add the test to |
| `test_class` | string | **Yes** | | Test class name. Prefix `EnvQueryTest_` is optional. |
| `purpose` | string | No | `Score` | Test purpose: `Filter`, `Score`, or `FilterAndScore` |
| `properties` | object | No | | Key-value pairs to set on the test via reflection |

### Built-in Test Classes

| Short Name | Full Class | Description |
|-----------|-----------|-------------|
| `Distance` | `EnvQueryTest_Distance` | Score/filter by distance to context |
| `Trace` | `EnvQueryTest_Trace` | Line-of-sight / visibility check |
| `Dot` | `EnvQueryTest_Dot` | Dot product between directions |
| `Pathfinding` | `EnvQueryTest_Pathfinding` | Path existence / cost / length |
| `PathfindingBatch` | `EnvQueryTest_PathfindingBatch` | Batch pathfinding test |
| `Overlap` | `EnvQueryTest_Overlap` | Collision overlap check |
| `GameplayTags` | `EnvQueryTest_GameplayTags` | Filter by gameplay tags |
| `Project` | `EnvQueryTest_Project` | Project points to navigation |
| `Volume` | `EnvQueryTest_Volume` | Check if inside a volume |
| `Random` | `EnvQueryTest_Random` | Random scoring (deprecated) |

## Response

```json
{
  "option_index": 0,
  "test_index": 0,
  "test_class": "EnvQueryTest_Distance",
  "purpose": "Score",
  "total_tests": 1,
  "properties_applied": [],
  "properties_failed": []
}
```

## Examples

### Add a Distance test (scoring)
```json
{ "tool": "add_eqs_test", "arguments": { "asset_path": "/Game/EQ_FindCover", "option_index": 0, "test_class": "Distance" } }
```

### Add a Trace test as filter
```json
{ "tool": "add_eqs_test", "arguments": { "asset_path": "/Game/EQ_FindCover", "option_index": 0, "test_class": "Trace", "purpose": "Filter" } }
```

## Error Cases

- `asset_path is required` — parameter missing or empty
- `option_index is required` — parameter missing
- `test_class is required` — parameter missing or empty
- `Invalid option_index: N (query has M options)` — index out of range
- `Test class not found: <name>` — no matching test class
- `Invalid purpose: <value>` — unsupported purpose string
- `EQS Query not found: <path>` — no query at that path
