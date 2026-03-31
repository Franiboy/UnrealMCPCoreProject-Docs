# create_anim_blueprint

Create an Animation Blueprint for a target skeleton. The blueprint is created with proper generated classes and compiled automatically. Optionally specify a parent class (default: `UAnimInstance`).

## Input

```json
{
  "name": "ABP_Character",
  "skeleton": "/Game/Characters/SK_Mannequin",
  "parent_class": "AnimInstance",
  "path": "/Game/Animations"
}
```

| Parameter      | Required | Default          | Description                                                       |
| -------------- | -------- | ---------------- | ----------------------------------------------------------------- |
| `name`         | **yes**  | —                | Name for the new Animation Blueprint.                             |
| `skeleton`     | **yes**  | —                | Content path to the target Skeleton asset.                        |
| `parent_class` | no       | `"AnimInstance"` | Parent class name or path. Must be `UAnimInstance` or a subclass. |
| `path`         | no       | `"/Game"`        | Content folder path for the new asset.                            |

## Output

| Field             | Type   | Description                              |
| ----------------- | ------ | ---------------------------------------- |
| `name`            | string | Created asset name                       |
| `asset_path`      | string | Full content path                        |
| `class`           | string | Always `"AnimBlueprint"`                 |
| `skeleton`        | string | Target skeleton path                     |
| `parent_class`    | string | Parent class path                        |
| `generated_class` | string | Generated class path (for use in actors) |

## Examples

### Create with defaults

```json
{
  "name": "ABP_Character",
  "skeleton": "/Game/Characters/SK_Mannequin"
}
```

### Create with custom parent class

```json
{
  "name": "ABP_Enemy",
  "skeleton": "/Game/Characters/SK_Enemy",
  "parent_class": "AnimInstance",
  "path": "/Game/Animations/Blueprints"
}
```

## Errors

| Condition                       | `isError` | Message                                              |
| ------------------------------- | --------- | ---------------------------------------------------- |
| Missing `name`                  | `true`    | `Missing required parameter 'name'`                  |
| Missing `skeleton`              | `true`    | `Missing required parameter 'skeleton'`              |
| Skeleton not found              | `true`    | `Skeleton not found or not a Skeleton: '...'`        |
| Parent class not found          | `true`    | `Parent class '...' not found.`                      |
| Parent not subclass of AnimInstance | `true` | `Class '...' is not a subclass of UAnimInstance.`    |
| Asset already exists            | `true`    | `An asset already exists at '...'`                   |
| Invalid path                    | `true`    | `Invalid path '...'. Must start with /Game or /Engine.` |
