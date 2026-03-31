# rename_blueprint

Rename or move a Blueprint asset. Unreal automatically creates redirectors so that existing references keep working.

## Input

```json
{
  "asset_path": "/Game/BP_OldName",
  "new_name": "BP_NewName",
  "new_path": "/Game/Blueprints"
}
```

| Parameter    | Required | Description                                                        |
| ------------ | -------- | ------------------------------------------------------------------ |
| `asset_path` | yes      | Current asset path of the Blueprint to rename                      |
| `new_name`   | yes      | New name for the Blueprint (without path)                          |
| `new_path`   | no       | New folder path. If omitted, the Blueprint stays in its current folder |

## Output

```json
{
  "old_name": "BP_OldName",
  "old_asset_path": "/Game/BP_OldName",
  "new_name": "BP_NewName",
  "new_asset_path": "/Game/Blueprints/BP_NewName",
  "new_folder": "/Game/Blueprints",
  "success": true
}
```

## Notes

- If the target name already exists, returns an error.
- Redirectors are created automatically so existing references continue to work.
- Undo renames the asset back to its original name and path.
