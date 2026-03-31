# set_project_settings

Write or modify project configuration in Default*.ini files. Changes are flushed to disk by default. Use this to set physics, rendering, navigation, and other project-level settings.

## Input

```json
{
  "section": "/Script/Engine.PhysicsSettings",
  "key": "DefaultGravityZ",
  "value": "-980.0"
}
```

| Parameter     | Required | Default         | Description                                                                 |
| ------------- | -------- | --------------- | --------------------------------------------------------------------------- |
| `section`     | **yes**  | —               | INI section name (e.g. `/Script/Engine.PhysicsSettings`).                   |
| `key`         | **yes**  | —               | Key within the section to set.                                              |
| `value`       | **yes**  | —               | Value to write. All values are stored as strings in INI files.              |
| `config_file` | no       | `DefaultEngine` | Config file alias: `DefaultEngine`, `DefaultGame`, `DefaultEditor`, `DefaultInput`. |
| `flush`       | no       | `true`          | Flush changes to disk immediately.                                          |

## Output

| Field            | Type    | Description                                 |
| ---------------- | ------- | ------------------------------------------- |
| `config_file`    | string  | Resolved config file path                   |
| `section`        | string  | Section written to                          |
| `key`            | string  | Key written                                 |
| `value`          | string  | Value written                               |
| `flushed`        | boolean | Whether changes were flushed to disk        |
| `previous_value` | string  | Previous value (if key existed before)      |
| `new_key`        | boolean | `true` if the key was newly created         |

## Examples

### Set a physics setting

```json
{
  "section": "/Script/Engine.PhysicsSettings",
  "key": "DefaultGravityZ",
  "value": "-980.0"
}
```

### Set a game setting

```json
{
  "config_file": "DefaultGame",
  "section": "/Script/EngineSettings.GeneralProjectSettings",
  "key": "ProjectName",
  "value": "My Awesome Game"
}
```

### Set without flushing to disk

```json
{
  "section": "/Script/Engine.RendererSettings",
  "key": "r.DefaultFeature.AntiAliasing",
  "value": "2",
  "flush": false
}
```

## Errors

| Condition         | Error message          |
| ----------------- | ---------------------- |
| Missing `section` | `section is required`  |
| Missing `key`     | `key is required`      |
| Missing `value`   | `value is required`    |
