# live_compile

Trigger Live Coding (hot reload) to recompile C++ code while the editor is running. Returns the compilation result and current Live Coding status.

By default the compile is **asynchronous** — it returns immediately with `in_progress`. Set `wait=true` to block until compilation finishes. Call with `status_only=true` to check current state without triggering a compile.

> **Platform:** Windows only. On other platforms the tool returns `not_available`.

## Input

```json
{
  "wait": true
}
```

| Parameter     | Required | Default | Description                                                                 |
| ------------- | -------- | ------- | --------------------------------------------------------------------------- |
| `wait`        | no       | `false` | If true, block until compilation finishes and return the final result.      |
| `status_only` | no       | `false` | If true, only return current Live Coding status without triggering a compile. |

## Output

| Field       | Type    | Description                                                                 |
| ----------- | ------- | --------------------------------------------------------------------------- |
| `result`    | string  | One of: `success`, `no_changes`, `in_progress`, `already_compiling`, `not_started`, `failure`, `cancelled`, `not_available`, `not_enabled`, `status` |
| `enabled`   | boolean | Whether Live Coding is enabled for this session                             |
| `started`   | boolean | Whether the Live Coding console has started (present when compiling)        |
| `compiling` | boolean | Whether a compilation is currently in progress                              |
| `message`   | string  | Human-readable description of the result                                    |

## Examples

### Check Live Coding status

```json
{
  "status_only": true
}
```

### Trigger async compile (fire-and-forget)

```json
{}
```

### Trigger synchronous compile (wait for result)

```json
{
  "wait": true
}
```

## Errors

This tool always returns `bSuccess = true` with a descriptive `result` string rather than MCP errors. Check the `result` field to determine the outcome:

| `result` value      | Meaning                                                         |
| ------------------- | --------------------------------------------------------------- |
| `success`           | Compilation succeeded — changes patched into the running editor |
| `no_changes`        | No source changes detected                                     |
| `in_progress`       | Async compile started; poll with `status_only=true`             |
| `already_compiling` | A prior compile request is still active                         |
| `not_started`       | Live Coding console could not be started                        |
| `failure`           | Compilation failed — check Output Log for errors                |
| `cancelled`         | Compilation was cancelled                                       |
| `not_available`     | Live Coding module not loaded or platform not supported         |
| `not_enabled`       | Failed to enable Live Coding for this session                   |
| `status`            | Status-only query — no compile was triggered                    |
