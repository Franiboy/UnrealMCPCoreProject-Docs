# get_editor_preferences

Read editor configuration settings from INI files. If only `section` is provided, returns all key=value pairs in that section. If both `section` and `key` are provided, returns the single value.

## Input

```json
{
  "section": "/Script/UnrealEd.EditorPerProjectUserSettings"
}
```

| Parameter     | Required | Default                          | Description                                                                 |
| ------------- | -------- | -------------------------------- | --------------------------------------------------------------------------- |
| `section`     | **yes**  | —                                | INI section name (e.g. `/Script/UnrealEd.EditorPerProjectUserSettings`).    |
| `key`         | no       | —                                | Key within the section. If omitted, all keys in the section are returned.   |
| `config_file` | no       | `EditorPerProjectUserSettings`   | Config file alias: `Editor`, `EditorPerProjectUserSettings`, `Engine`, `Game`, `Input`. |

## Output

### When `key` is provided

| Field         | Type    | Description                         |
| ------------- | ------- | ----------------------------------- |
| `config_file` | string  | Resolved config file path           |
| `section`     | string  | Section name queried                |
| `key`         | string  | Key queried                         |
| `found`       | boolean | Whether the key exists              |
| `value`       | string  | Value (only present if found)       |

### When `key` is omitted (section dump)

| Field            | Type    | Description                         |
| ---------------- | ------- | ----------------------------------- |
| `config_file`    | string  | Resolved config file path           |
| `section`        | string  | Section name queried                |
| `section_exists` | boolean | Whether the section exists          |
| `values`         | object  | Key-value pairs in the section      |
| `num_keys`       | number  | Number of keys in the section       |

## Examples

### Read all keys in a section

```json
{
  "section": "URL",
  "config_file": "Engine"
}
```

### Read a specific key

```json
{
  "section": "URL",
  "key": "GameName",
  "config_file": "Engine"
}
```

### Read from default config file

```json
{
  "section": "/Script/UnrealEd.EditorPerProjectUserSettings"
}
```

## Errors

| Condition           | Error message        |
| ------------------- | -------------------- |
| Missing `section`   | `section is required`|
