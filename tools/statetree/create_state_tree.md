# create_state_tree

**Category:** State Tree  
**Source:** `Private/StateTree/MCPStateTreeTools_Edit.cpp`

## Description

Creates a new State Tree asset with a specified schema type. A default "Root" state is automatically added. The tree is compiled and saved after creation.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `name` | string | **Yes** | | Name of the State Tree asset |
| `path` | string | No | `/Game` | Content path for the asset |
| `schema` | string | No | `StateTreeComponentSchema` | Schema class name (e.g. `StateTreeComponentSchema`) |

## Response

```json
{
  "name": "ST_EnemyAI",
  "asset_path": "/Game/AI/ST_EnemyAI.ST_EnemyAI",
  "schema": "StateTreeComponentSchema",
  "state_count": 1
}
```

## Examples

### Create a basic state tree
```json
{ "tool": "create_state_tree", "arguments": { "name": "ST_EnemyAI" } }
```

### Create in a subfolder with explicit schema
```json
{ "tool": "create_state_tree", "arguments": { "name": "ST_Patrol", "path": "/Game/AI", "schema": "StateTreeComponentSchema" } }
```

## Error Cases

- `name` is required
- Returns error if an asset with the same name already exists at the path
- Returns error if the schema class is not found
