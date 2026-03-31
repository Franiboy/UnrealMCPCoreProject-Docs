# duplicate_blueprint

Duplicate a Blueprint asset to a new name and optionally a different folder. The original Blueprint remains unchanged.

## Input

```json
{
  "asset_path": "/Game/BP_MyActor",
  "new_name": "BP_MyActor_Copy",
  "new_path": "/Game/Copies"
}
```

| Parameter    | Required | Description                                                           |
| ------------ | -------- | --------------------------------------------------------------------- |
| `asset_path` | yes      | Asset path of the source Blueprint to duplicate                       |
| `new_name`   | yes      | Name for the duplicated Blueprint                                     |
| `new_path`   | no       | Folder path for the duplicate. If omitted, uses the same folder as the source |

## Output

```json
{
  "source_asset_path": "/Game/BP_MyActor",
  "name": "BP_MyActor_Copy",
  "asset_path": "/Game/Copies/BP_MyActor_Copy",
  "blueprint_type": "Normal",
  "compile_status": "UpToDate",
  "parent_class": "Actor",
  "parent_class_path": "/Script/Engine.Actor",
  "generated_class": "/Game/Copies/BP_MyActor_Copy.BP_MyActor_Copy_C"
}
```

## Notes

- If the target name already exists, returns an error.
- The duplicate inherits the same parent class as the source.
- The duplicate is compiled automatically after creation.
- Undo deletes the duplicated asset.
