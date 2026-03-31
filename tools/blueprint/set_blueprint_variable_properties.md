# set_blueprint_variable_properties

Change properties of an existing Blueprint variable. Only the provided fields are changed; others are left untouched. The Blueprint is recompiled after changes.

## Input

```json
{
  "asset_path": "/Game/BP_MyActor",
  "variable_name": "Health",
  "category": "Combat",
  "default_value": "200.0",
  "replication": "Replicated",
  "instance_editable": false
}
```

| Parameter            | Required | Description                                                              |
| -------------------- | -------- | ------------------------------------------------------------------------ |
| `asset_path`         | yes      | Asset path of the Blueprint (e.g. `/Game/Blueprints/BP_MyActor`)         |
| `variable_name`      | yes      | Name of the variable to modify                                           |
| `variable_type`      | no       | New type (e.g. `int`, `FVector`, `Array<float>`). See `add_blueprint_variable` for supported types |
| `default_value`      | no       | New default value as string (e.g. `100.0`, `true`, `(X=1,Y=2,Z=3)`)     |
| `category`           | no       | New category name for the Details panel                                  |
| `tooltip`            | no       | New tooltip (empty string removes it)                                    |
| `instance_editable`  | no       | Editable per-instance in Details panel                                   |
| `blueprint_read_only`| no       | Read-only in Blueprints                                                  |
| `expose_on_spawn`    | no       | Expose as pin on Spawn Actor node                                        |
| `private`            | no       | Private (not accessible from other Blueprints)                           |
| `transient`          | no       | Transient (not saved to disk)                                            |
| `save_game`          | no       | Included in save games                                                   |
| `replication`        | no       | Replication mode: `None`, `Replicated`, or `RepNotify`                   |
| `rep_notify_func`    | no       | Name of the RepNotify function (e.g. `OnRep_Health`)                     |

At least one optional parameter must be provided.

## Output

```json
{
  "asset_path": "/Game/BP_MyActor",
  "variable_name": "Health",
  "changes_applied": 3,
  "changes": {
    "category": "Combat",
    "default_value": "200.0",
    "replication": "Replicated"
  },
  "undo": {
    "action": "restore_variable_properties",
    "asset_path": "/Game/BP_MyActor",
    "variable_name": "Health",
    "old_category": "Stats",
    "old_default_value": "100.0",
    "old_replication": "None"
  }
}
```

## Notes

- Returns an error if the variable does not exist, no properties are provided, the replication mode is invalid, or the type string cannot be parsed.
- When changing `variable_type`, the engine's `ChangeMemberVariableType` is used; the default value may be reset.
- Undo restores all changed properties to their previous values.
- No reflection needed -- all properties are read/written on the `FBPVariableDescription` struct directly.
