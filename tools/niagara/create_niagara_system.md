# create_niagara_system

Create a new Niagara System asset (empty or from a template system).

## Input

```json
{
  "name": "NS_Fire",
  "path": "/Game/FX",
  "template": "/Niagara/DefaultAssets/Templates/Systems/SimpleExplosion"
}
```

| Parameter  | Required | Description                                                                 |
| ---------- | -------- | --------------------------------------------------------------------------- |
| `name`     | **yes**  | Name for the new Niagara System asset                                       |
| `path`     | no       | Content folder path (default: `/Game`)                                      |
| `template` | no       | Asset path of an existing Niagara System to use as template (omit for empty)|

## Output

```json
{
  "name": "NS_Fire",
  "asset_path": "/Game/FX/NS_Fire",
  "class": "NiagaraSystem",
  "from_template": true,
  "template": "/Niagara/DefaultAssets/Templates/Systems/SimpleExplosion",
  "emitter_count": 3,
  "is_valid": true
}
```

| Field           | Type    | Description                                       |
| --------------- | ------- | ------------------------------------------------- |
| `name`          | string  | Asset name                                        |
| `asset_path`    | string  | Full content path                                 |
| `class`         | string  | Always `NiagaraSystem`                            |
| `from_template` | boolean | Whether the system was created from a template    |
| `template`      | string  | Template path (only present when `from_template`) |
| `emitter_count` | number  | Number of emitters in the new system              |
| `is_valid`      | boolean | Whether the system is valid                       |

## Examples

### Create an empty system

```json
{"name": "NS_MyEffect"}
```

### Create from a template in a subfolder

```json
{
  "name": "NS_Explosion",
  "path": "/Game/FX/Particles",
  "template": "/Niagara/DefaultAssets/Templates/Systems/SimpleExplosion"
}
```

## Errors

| Condition                        | Message                                 |
| -------------------------------- | --------------------------------------- |
| `name` missing or empty          | Missing required parameter 'name'       |
| `path` not `/Game` or `/Engine`  | path must start with /Game or /Engine   |
| Asset already exists at path     | An asset already exists at '...'        |
| Template system not found        | Template system not found: ...          |
