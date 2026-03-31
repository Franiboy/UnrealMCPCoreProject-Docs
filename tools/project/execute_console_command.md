# execute_console_command

Run an Unreal console command in the editor and capture the output. Uses `GEditor->Exec()` with a `FStringOutputDevice` to capture printed results.

## Input

```json
{
  "command": "stat unit"
}
```

| Parameter | Required | Default | Description                                      |
| --------- | -------- | ------- | ------------------------------------------------ |
| `command` | **yes**  | —       | The console command string to execute.           |

## Output

| Field        | Type    | Description                                              |
| ------------ | ------- | -------------------------------------------------------- |
| `command`    | string  | The command that was executed                             |
| `recognized` | boolean | Whether the engine recognized the command                |
| `output`     | string  | Combined output text from the command                    |
| `lines`      | array   | Output split into individual lines (strings)             |
| `num_lines`  | number  | Number of output lines                                   |

## Examples

### Run a stat command

```json
{
  "command": "stat unit"
}
```

### List objects

```json
{
  "command": "obj list"
}
```

### Run a custom console command

```json
{
  "command": "r.SetRes 1920x1080"
}
```

## Errors

| Condition                   | Error message                  |
| --------------------------- | ------------------------------ |
| Missing `command` parameter | `command is required`          |
| Empty `command` string      | `command must not be empty`    |
| Editor not available        | `Editor not available`         |
