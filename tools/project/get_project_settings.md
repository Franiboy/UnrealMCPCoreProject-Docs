# get_project_settings

Read project configuration from Default*.ini files (DefaultEngine.ini, DefaultGame.ini, DefaultEditor.ini, DefaultInput.ini). Returns key-value pairs for a section, or a single value if a key is specified. Use `list_sections=true` to enumerate all sections.

## Input

```json
{
  "section": "/Script/Engine.PhysicsSettings"
}
```

| Parameter       | Required | Default           | Description                                                                                   |
| --------------- | -------- | ----------------- | --------------------------------------------------------------------------------------------- |
| `config_file`   | no       | `DefaultEngine`   | Config file alias: `DefaultEngine`, `DefaultGame`, `DefaultEditor`, `DefaultInput`. Also accepts a filename directly. |
| `section`       | cond.    | —                 | INI section name. Required unless `list_sections` is true.                                    |
| `key`           | no       | —                 | Key within the section. If omitted, all keys in the section are returned.                     |
| `list_sections` | no       | `false`           | If true, return all section names in the config file instead of reading values.               |

## Output

### When `list_sections` is true

| Field         | Type   | Description                       |
| ------------- | ------ | --------------------------------- |
| `config_file` | string | Resolved config file path         |
| `count`       | number | Number of sections                |
| `sections`    | array  | Array of section name strings     |

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

### List all sections in DefaultEngine.ini

```json
{
  "list_sections": true
}
```

### Read a physics setting

```json
{
  "section": "/Script/Engine.PhysicsSettings",
  "key": "DefaultGravityZ"
}
```

### Read all rendering settings

```json
{
  "section": "/Script/Engine.RendererSettings"
}
```

### Read from DefaultGame.ini

```json
{
  "config_file": "DefaultGame",
  "section": "/Script/EngineSettings.GeneralProjectSettings",
  "key": "ProjectID"
}
```

## Errors

| Condition                                  | Error message                                      |
| ------------------------------------------ | -------------------------------------------------- |
| Missing `section` and `list_sections` false | `section is required (or set list_sections=true)` |
