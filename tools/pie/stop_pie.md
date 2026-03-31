# stop_pie

Stop the current Play In Editor session.

## Parameters

None.

## Example

**Request:**
```json
{
  "tool": "stop_pie",
  "arguments": {}
}
```

**Response:**
```json
{
  "stopped": true,
  "status": "PIE session stop requested"
}
```

## Notes

- Returns an error if no PIE session is currently running.
- Stop is deferred to the next tick; the session may not be fully stopped immediately.
- Also cancels any queued play session requests.
