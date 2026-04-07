# list_tools

List all registered MCP tools with their name, description, and category. This is the `tools/call` equivalent of the MCP `tools/list` protocol method — useful when an agent wants to discover available tools via a normal tool call.

## Input

```json
{
  "category": "Blueprint"
}
```

| Parameter  | Required | Default | Description                                                                 |
| ---------- | -------- | ------- | --------------------------------------------------------------------------- |
| `category` | no       | —       | Optional category filter (e.g. `Blueprint`, `Asset`, `Level`). Only tools in this category are returned. Case-insensitive. |

## Output

| Field   | Type   | Description                                      |
| ------- | ------ | ------------------------------------------------ |
| `count` | number | Number of tools returned                         |
| `tools` | array  | Array of tool objects, sorted by category + name |

Each tool object:

| Field         | Type   | Description                    |
| ------------- | ------ | ------------------------------ |
| `name`        | string | Tool name                      |
| `description` | string | Tool description                |
| `category`    | string | Category label (if set)        |

## Examples

### List all tools

```json
{}
```

### List only Blueprint tools

```json
{
  "category": "Blueprint"
}
```

### List only Asset tools

```json
{
  "category": "Asset"
}
```

## Errors

| Condition                    | Error message                      |
| ---------------------------- | ---------------------------------- |
| Module not loaded            | `UnrealMCPCore module not loaded.` |
| Protocol handler unavailable | `Protocol handler not available.`  |
