# create_material

Create a new UMaterial asset with configurable domain, blend mode, shading model, and two-sided rendering.

## Input

```json
{
  "name": "M_MyMaterial",
  "path": "/Game/Materials",
  "domain": "Surface",
  "blend_mode": "Opaque",
  "shading_model": "DefaultLit",
  "two_sided": false
}
```

| Parameter       | Required | Description                                                                                                   |
| --------------- | -------- | ------------------------------------------------------------------------------------------------------------- |
| `name`          | **yes**  | Material asset name (e.g. `M_MyMaterial`)                                                                     |
| `path`          | no       | Content folder path (default: `/Game`)                                                                        |
| `domain`        | no       | Material domain (default: `Surface`). See values below                                                        |
| `blend_mode`    | no       | Blend mode (default: `Opaque`). See values below                                                              |
| `shading_model` | no       | Shading model (default: `DefaultLit`). See values below                                                       |
| `two_sided`     | no       | Enable two-sided rendering (default: `false`)                                                                 |

### Domain Values

`Surface`, `DeferredDecal`, `LightFunction`, `Volume`, `PostProcess`, `UI`, `RuntimeVirtualTexture`

### Blend Mode Values

`Opaque`, `Masked`, `Translucent`, `Additive`, `Modulate`, `AlphaComposite`, `AlphaHoldout`

### Shading Model Values

`Unlit`, `DefaultLit`, `Subsurface`, `PreintegratedSkin`, `ClearCoat`, `SubsurfaceProfile`, `TwoSidedFoliage`, `Hair`, `Cloth`, `Eye`, `SingleLayerWater`, `ThinTranslucent`

## Output

```json
{
  "name": "M_MyMaterial",
  "asset_path": "/Game/Materials/M_MyMaterial",
  "class": "Material",
  "domain": "Surface",
  "blend_mode": "Opaque",
  "shading_model": "DefaultLit",
  "two_sided": false
}
```

| Field           | Type   | Description                                 |
| --------------- | ------ | ------------------------------------------- |
| `name`          | string | Created material asset name                 |
| `asset_path`    | string | Full content path to the new material       |
| `class`         | string | Always `Material`                           |
| `domain`        | string | Material domain that was set                |
| `blend_mode`    | string | Blend mode that was set                     |
| `shading_model` | string | Shading model that was set                  |
| `two_sided`     | bool   | Whether two-sided rendering is enabled      |

## Examples

### Create a basic opaque material

```json
{"name": "M_Floor"}
```

### Create a translucent glass material

```json
{
  "name": "M_Glass",
  "blend_mode": "Translucent",
  "shading_model": "DefaultLit"
}
```

### Create a post-process material

```json
{
  "name": "M_PP_Vignette",
  "domain": "PostProcess"
}
```

### Create a two-sided foliage material

```json
{
  "name": "M_Leaves",
  "blend_mode": "Masked",
  "shading_model": "TwoSidedFoliage",
  "two_sided": true
}
```

## Errors

| Condition              | `isError` | Message                                                 |
| ---------------------- | --------- | ------------------------------------------------------- |
| Missing `name`         | `true`    | `Missing required parameter 'name'`                     |
| Asset already exists   | `true`    | `An asset already exists at '/Game/...'`                |
| Invalid `domain`       | `true`    | `Invalid domain '...'. Valid: Surface, ...`             |
| Invalid `blend_mode`   | `true`    | `Invalid blend_mode '...'. Valid: Opaque, ...`          |
| Invalid `shading_model`| `true`    | `Invalid shading_model '...'. Valid: Unlit, ...`        |
| Invalid `path`         | `true`    | `Invalid path '...'. Must start with /Game or /Engine.` |
