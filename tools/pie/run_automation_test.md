# run_automation_test

List available Unreal Automation Tests or run a specific test by name.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | No | Full test name to run (e.g. `Project.Functional Tests.MyTest`). If omitted, lists all available tests. |
| `filter` | string | No | Filter test list by substring (case-insensitive). Only used when listing tests. |

## Example

**List tests:**
```json
{
  "tool": "run_automation_test",
  "arguments": {
    "filter": "System"
  }
}
```

**Response:**
```json
{
  "tests": [
    {
      "name": "System.Core.Math.Vector.Addition",
      "display_name": "Vector Addition",
      "flags": 4
    }
  ],
  "count": 1
}
```

**Run a test:**
```json
{
  "tool": "run_automation_test",
  "arguments": {
    "name": "System.Core.Math.Vector.Addition"
  }
}
```

**Response:**
```json
{
  "test_name": "System.Core.Math.Vector.Addition",
  "started": true,
  "completed": true
}
```

## Notes

- When `name` is omitted, lists all registered automation tests with optional `filter`.
- When `name` is provided, the test is started synchronously if possible.
- Complex latent tests may not complete within the iteration limit; check the `completed` field.
- Returns error if the named test is not found.
