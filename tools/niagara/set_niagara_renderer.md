# set_niagara_renderer

Configure renderer settings on an emitter within a Niagara System.

## Input

```json
{
  "system_path": "/Game/FX/NS_Fire",
  "emitter_name": "Sparks",
  "renderer_index": 0,
  "enabled": true,
  "material": "/Game/Materials/M_Spark",
  "properties": {
    "SortOrderHint": 5
  }
}
```

| Parameter        | Required | Description                                                     |
| ---------------- | -------- | --------------------------------------------------------------- |
| `system_path`    | **yes**  | Asset path of the Niagara System                                |
| `emitter_name`   | no       | Emitter name (auto-selected if system has one emitter)          |
| `renderer_index` | **yes**  | Renderer index within the emitter (0-based)                     |
| `enabled`        | no       | Enable or disable the renderer (boolean)                        |
| `material`       | no       | Asset path of the material to assign                            |
| `properties`     | no       | Additional properties as UPROPERTY name-value pairs (object)    |

At least one of `enabled`, `material`, or `properties` must be provided.

## Output

```json
{
  "system": "NS_Fire",
  "emitter_name": "Sparks",
  "renderer_index": 0,
  "renderer_type": "SpriteRenderer",
  "properties_set": 2,
  "updated": {
    "enabled": true,
    "SortOrderHint": "5"
  }
}
```

| Field            | Type   | Description                                    |
| ---------------- | ------ | ---------------------------------------------- |
| `system`         | string | System asset name                              |
| `emitter_name`   | string | Name of the emitter                            |
| `renderer_index` | number | Index of the modified renderer                 |
| `renderer_type`  | string | Renderer class (e.g. SpriteRenderer, MeshRenderer) |
| `properties_set` | number | Number of properties successfully set          |
| `updated`        | object | Key-value pairs of updated properties          |

## Common renderer types

| Class name (simplified) | Full UObject class                       |
| ----------------------- | ---------------------------------------- |
| SpriteRenderer          | UNiagaraSpriteRendererProperties         |
| MeshRenderer            | UNiagaraMeshRendererProperties           |
| RibbonRenderer          | UNiagaraRibbonRendererProperties         |
| LightRenderer           | UNiagaraLightRendererProperties          |

## Examples

### Disable a renderer

```json
{"system_path": "/Game/FX/NS_Fire", "emitter_name": "Sparks", "renderer_index": 0, "enabled": false}
```

### Assign a material

```json
{
  "system_path": "/Game/FX/NS_Fire",
  "emitter_name": "Sparks",
  "renderer_index": 0,
  "material": "/Game/Materials/M_Spark"
}
```

### Set arbitrary UPROPERTY values

```json
{
  "system_path": "/Game/FX/NS_Fire",
  "emitter_name": "Sparks",
  "renderer_index": 0,
  "properties": {
    "SortOrderHint": 10,
    "bGpuLowLatencyTranslucency": "True"
  }
}
```

## Errors

| Condition              | Message                                              |
| ---------------------- | ---------------------------------------------------- |
| Missing required       | system_path is required / renderer_index is required |
| System not found       | System not found: ...                                |
| Emitter not found      | Emitter '...' not found. Available: ...              |
| Index out of range     | renderer_index N out of range (0..M)                 |
| No properties set      | No properties were set                               |
