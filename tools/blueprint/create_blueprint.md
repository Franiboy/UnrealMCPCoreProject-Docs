# create_blueprint

Create a new Blueprint asset with a configurable parent class.

## Input

```json
{
  "name": "BP_NewActor",
  "path": "/Game/Blueprints",
  "parent_class": "Character"
}
```

| Parameter      | Required | Description                                    |
| -------------- | -------- | ---------------------------------------------- |
| `name`         | yes      | Blueprint name (must be unique in target path) |
| `path`         | no       | Package path (default: `/Game`)                |
| `parent_class` | no       | Parent class name (default: `Actor`). Accepts C++ class names (e.g. `Character`, `Pawn`), full class paths (e.g. `/Script/Engine.Character`), or Blueprint generated classes (e.g. `/Game/BP_Base.BP_Base_C`). |

## Output

```json
{
  "name": "BP_NewActor",
  "asset_path": "/Game/Blueprints/BP_NewActor",
  "blueprint_type": "Normal",
  "compile_status": "UpToDate",
  "parent_class": "Character",
  "parent_class_path": "/Script/Engine.Character",
  "generated_class": "/Game/Blueprints/BP_NewActor.BP_NewActor_C"
}
```
