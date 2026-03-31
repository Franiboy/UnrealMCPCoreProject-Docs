# set_actor_data_layer

Add or remove an actor from a Data Layer.

## Input

```json
{
  "actor_name": "MyActor",
  "data_layer_name": "DL_Gameplay",
  "action": "add"
}
```

|| Parameter         | Required | Default  | Description                              |
|| ----------------- | -------- | -------- | ---------------------------------------- |
|| `actor_name`      | **yes**  | ---      | Name or label of the actor.              |
|| `data_layer_name` | **yes**  | ---      | Name of the Data Layer.                  |
|| `action`          | no       | `"add"`  | `"add"` or `"remove"`.                   |

## Output

|| Field             | Type    | Description                          |
|| ----------------- | ------- | ------------------------------------ |
|| `actor_name`      | string  | Name of the actor                    |
|| `data_layer_name` | string  | Name of the data layer               |
|| `action`          | string  | Action performed (`"add"` or `"remove"`) |
|| `success`         | boolean | Whether the operation succeeded      |

## Examples

### Add actor to data layer

```json
{
  "actor_name": "BP_Enemy_01",
  "data_layer_name": "DL_Gameplay"
}
```

### Remove actor from data layer

```json
{
  "actor_name": "BP_Enemy_01",
  "data_layer_name": "DL_Gameplay",
  "action": "remove"
}
```

## Errors

|| Condition                  | `isError` | Message                              |
|| -------------------------- | --------- | ------------------------------------ |
|| Missing `actor_name`       | true      | 'actor_name' is required             |
|| Missing `data_layer_name`  | true      | 'data_layer_name' is required        |
|| Actor not found            | true      | Actor not found: ...                 |
|| Data layer not found       | true      | Data Layer not found: ...            |
|| Invalid action             | true      | Unknown action: ...                  |
|| No editor world            | true      | No editor world available            |
