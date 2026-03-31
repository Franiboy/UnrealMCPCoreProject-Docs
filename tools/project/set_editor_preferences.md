# set_editor_preferences

Write an editor configuration setting to an INI file. Changes are flushed to disk by default.

## Input

```json
{
  "section": "/Script/MyPlugin.MySettings",
  "key": "bEnableFeature",
  "value": "True"
}
```

| Parameter     | Required | Default                          | Description                                                                 |
| ------------- | -------- | -------------------------------- | --------------------------------------------------------------------------- |
| `section`     | **yes**  | —                                | INI section name (e.g. `/Script/UnrealEd.EditorPerProjectUserSettings`).    |
| `key`         | **yes**  | —                                | Key within the section to set.                                              |
| `value`       | **yes**  | —                                | Value to write. All values are stored as strings in INI files.              |
| `config_file` | no       | `EditorPerProjectUserSettings`   | Config file alias: `Editor`, `EditorPerProjectUserSettings`, `Engine`, `Game`, `Input`. |
| `flush`       | no       | `true`                           | Flush changes to disk immediately.                                          |

## Output

| Field            | Type    | Description                              |
| ---------------- | ------- | ---------------------------------------- |
| `config_file`    | string  | Resolved config file path                |
| `section`        | string  | Section written to                       |
| `key`            | string  | Key written                              |
| `value`          | string  | Value written                            |
| `flushed`        | boolean | Whether changes were flushed to disk     |
| `previous_value` | string  | Previous value (if key already existed)  |
| `new_key`        | boolean | `true` if this is a newly created key    |

## Examples

### Write a setting and flush

```json
{
  "section": "/Script/MyPlugin.MySettings",
  "key": "MaxRetries",
  "value": "5"
}
```

### Write without flushing to disk

```json
{
  "section": "/Script/MyPlugin.MySettings",
  "key": "TempFlag",
  "value": "True",
  "flush": false
}
```

### Write to the Engine config

```json
{
  "section": "/Script/Engine.RendererSettings",
  "key": "r.DefaultFeature.Bloom",
  "value": "True",
  "config_file": "Engine"
}
```

## Errors

| Condition         | Error message          |
| ----------------- | ---------------------- |
| Missing `section` | `section is required`  |
| Missing `key`     | `key is required`      |
| Missing `value`   | `value is required`    |
