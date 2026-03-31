# list_gameplay_tags

List all gameplay tags defined in the project. Optionally filter by prefix to see only matching tags. Returns tag name, whether it is explicitly defined, and structural info.

## Input

```json
{}
```

| Parameter       | Required | Default | Description                                                                      |
| --------------- | -------- | ------- | -------------------------------------------------------------------------------- |
| `prefix`        | no       | —       | Filter tags by prefix (e.g. `Ability.` returns only tags starting with `Ability.`). |
| `explicit_only` | no       | `false` | If true, only return explicitly defined tags (not implicit parent tags).          |
| `limit`         | no       | all     | Maximum number of tags to return.                                                |

## Output

| Field           | Type    | Description                                       |
| --------------- | ------- | ------------------------------------------------- |
| `count`         | number  | Number of tags returned                           |
| `tags`          | array   | Array of tag objects                               |
| `prefix`        | string  | Prefix filter applied (if any)                    |
| `explicit_only` | boolean | Whether explicit-only filter was applied          |

### Tag object

| Field         | Type    | Description                                     |
| ------------- | ------- | ----------------------------------------------- |
| `tag`         | string  | Full tag string (e.g. `Ability.Attack.Melee`)   |
| `explicit`    | boolean | Whether the tag is explicitly defined            |
| `simple_name` | string  | Leaf portion of the tag name                     |
| `restricted`  | boolean | Whether the tag is restricted (only if true)     |
| `parent`      | string  | Direct parent tag (if any)                       |

## Examples

### List all tags

```json
{}
```

### Filter by prefix

```json
{
  "prefix": "Ability."
}
```

### Only explicit tags with limit

```json
{
  "explicit_only": true,
  "limit": 20
}
```

## Errors

This tool does not produce errors; an empty project returns `count: 0` with an empty tags array.
