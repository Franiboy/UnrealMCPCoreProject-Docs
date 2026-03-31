# inject_input

Simulate keyboard, mouse, or gamepad input, or execute a console command in the PIE world.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `key` | string | No* | Key name (e.g. `W`, `SpaceBar`, `LeftMouseButton`, `Enter`, `Escape`) |
| `event` | string | No | Event type: `press` (default), `release`, or `click` (press+release) |
| `command` | string | No* | Console command to execute in the PIE world |

\* Either `key` or `command` is required.

## Example

**Key input:**
```json
{
  "tool": "inject_input",
  "arguments": {
    "key": "SpaceBar",
    "event": "click"
  }
}
```

**Response:**
```json
{
  "key": "SpaceBar",
  "event": "click",
  "injected": true
}
```

**Console command:**
```json
{
  "tool": "inject_input",
  "arguments": {
    "command": "stat fps"
  }
}
```

**Response:**
```json
{
  "command": "stat fps",
  "executed": true
}
```

## Notes

- Key input is processed through Slate's input pipeline (`ProcessKeyDownEvent` / `ProcessKeyUpEvent`).
- Console commands require an active PIE session; returns error if PIE is not running.
- Key names follow Unreal Engine's `FKey` naming convention.
- `click` sends both a press and release event.
