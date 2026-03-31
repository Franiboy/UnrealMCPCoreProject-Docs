# create_niagara_emitter

Create a new Niagara Emitter asset (empty or from a template emitter).

## Input

```json
{
  "name": "NE_Sparks",
  "path": "/Game/FX",
  "template": "/Niagara/DefaultAssets/Templates/Emitters/Fountain",
  "add_defaults": true
}
```

| Parameter      | Required | Description                                                                                    |
| -------------- | -------- | ---------------------------------------------------------------------------------------------- |
| `name`         | **yes**  | Name for the new Niagara Emitter asset                                                         |
| `path`         | no       | Content folder path (default: `/Game`)                                                         |
| `template`     | no       | Asset path of an existing Niagara Emitter to use as template (omit for empty)                  |
| `add_defaults` | no       | Add default modules and renderers to a new empty emitter (default: `true`, ignored with template) |

## Output

```json
{
  "name": "NE_Sparks",
  "asset_path": "/Game/FX/NE_Sparks",
  "class": "NiagaraEmitter",
  "from_template": false,
  "add_defaults": true,
  "unique_name": "NE_Sparks",
  "sim_target": "CPU",
  "renderer_count": 1
}
```

| Field            | Type    | Description                                        |
| ---------------- | ------- | -------------------------------------------------- |
| `name`           | string  | Asset name                                         |
| `asset_path`     | string  | Full content path                                  |
| `class`          | string  | Always `NiagaraEmitter`                            |
| `from_template`  | boolean | Whether the emitter was created from a template    |
| `template`       | string  | Template path (only present when `from_template`)  |
| `add_defaults`   | boolean | Whether default modules/renderers were added       |
| `unique_name`    | string  | Unique emitter name                                |
| `sim_target`     | string  | `CPU` or `GPU`                                     |
| `renderer_count` | number  | Number of renderers in the emitter                 |

## Examples

### Create an empty emitter with default modules

```json
{"name": "NE_MyEmitter"}
```

### Create a bare emitter (no defaults)

```json
{"name": "NE_Bare", "add_defaults": false}
```

### Create from a template in a subfolder

```json
{
  "name": "NE_Fountain",
  "path": "/Game/FX/Emitters",
  "template": "/Niagara/DefaultAssets/Templates/Emitters/Fountain"
}
```

## Errors

| Condition                        | Message                                 |
| -------------------------------- | --------------------------------------- |
| `name` missing or empty          | Missing required parameter 'name'       |
| `path` not `/Game` or `/Engine`  | path must start with /Game or /Engine   |
| Asset already exists at path     | An asset already exists at '...'        |
| Template emitter not found       | Template emitter not found: ...         |
