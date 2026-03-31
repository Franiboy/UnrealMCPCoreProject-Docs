# remove_event_dispatcher

Remove an event dispatcher (multicast delegate) from a Blueprint by name. Returns the removed dispatcher's parameters for undo support.

## Input

```json
{
  "asset_path": "/Game/BP_MyActor",
  "dispatcher_name": "OnHealthChanged"
}
```

| Parameter         | Required | Description                                                   |
| ----------------- | -------- | ------------------------------------------------------------- |
| `asset_path`      | yes      | Blueprint asset path                                          |
| `dispatcher_name` | yes      | Name of the event dispatcher to remove                        |

## Output

```json
{
  "asset_path": "/Game/BP_MyActor",
  "dispatcher_name": "OnHealthChanged",
  "parameters": [
    {"name": "NewHealth", "type": "real (float)"},
    {"name": "DamageCauser", "type": "name"}
  ]
}
```

| Field              | Description                                              |
| ------------------ | -------------------------------------------------------- |
| `asset_path`       | The Blueprint that was modified                          |
| `dispatcher_name`  | Name of the removed event dispatcher                     |
| `parameters`       | Array of the dispatcher's parameters (captured before removal) |

## Errors

| Scenario                    | Error message contains          |
| --------------------------- | ------------------------------- |
| Blueprint not found         | `"not found at path"`           |
| Dispatcher not found        | `"not found in"`                |
| Missing `dispatcher_name`   | `"dispatcher_name"`             |

## Notes

- The Blueprint is compiled automatically after removal.
- Only removes variables with `PC_MCDelegate` pin category (event dispatchers), not regular variables.
- Undo re-creates the dispatcher with its original parameters via `AddMemberVariable` + delegate signature graph creation.
