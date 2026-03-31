# create_eqs_query

**Category:** EQS (Environment Query System)  
**Source:** `Private/EQS/MCPEQSTools_Edit.cpp`

## Description

Creates a new, empty EQS Query template asset. The query starts with zero options; use `add_eqs_generator` to add generators and `add_eqs_test` to add tests.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `name` | string | **Yes** | | Name of the EQS Query asset |
| `path` | string | No | `/Game` | Content path to create the asset in |

## Response

```json
{
  "name": "EQ_FindCover",
  "asset_path": "/Game/EQ_FindCover.EQ_FindCover",
  "option_count": 0
}
```

## Examples

### Create a basic EQS query
```json
{ "tool": "create_eqs_query", "arguments": { "name": "EQ_FindCover" } }
```

### Create in a subfolder
```json
{ "tool": "create_eqs_query", "arguments": { "name": "EQ_Patrol", "path": "/Game/AI" } }
```

## Error Cases

- `name is required` — parameter missing or empty
- `EQS Query already exists: <path>` — asset already exists at that path
- `Failed to create package: <path>` — package creation failed
