# set_material_properties

Modify properties on an existing base UMaterial asset. Change domain, blend mode, shading model, two-sided rendering, and opacity mask clip value. Only works on base materials (UMaterial), not material instances. The material is recompiled and saved to disk after modification.

## Input

```json
{
  "asset_path": "/Game/Materials/M_MyMaterial",
  "domain": "Surface",
  "blend_mode": "Masked",
  "shading_model": "DefaultLit",
  "two_sided": true,
  "opacity_mask_clip_value": 0.5
}
```

| Parameter                | Required | Description                                                                                          |
| ------------------------ | -------- | ---------------------------------------------------------------------------------------------------- |
| `asset_path`             | **yes**  | Content path to the base material asset (e.g. `/Game/Materials/M_MyMaterial`)                        |
| `domain`                 | no       | Material domain. Valid: `Surface`, `DeferredDecal`, `LightFunction`, `Volume`, `PostProcess`, `UI`, `RuntimeVirtualTexture` |
| `blend_mode`             | no       | Blend mode. Valid: `Opaque`, `Masked`, `Translucent`, `Additive`, `Modulate`, `AlphaComposite`, `AlphaHoldout` |
| `shading_model`          | no       | Shading model. Valid: `Unlit`, `DefaultLit`, `Subsurface`, `PreintegratedSkin`, `ClearCoat`, `SubsurfaceProfile`, `TwoSidedFoliage`, `Hair`, `Cloth`, `Eye`, `SingleLayerWater`, `ThinTranslucent` |
| `two_sided`              | no       | Enable or disable two-sided rendering                                                                |
| `opacity_mask_clip_value`| no       | Opacity mask clip value (0.0–1.0). Only relevant when blend_mode is `Masked`                         |

At least one optional property must be provided.

## Output

```json
{
  "name": "M_MyMaterial",
  "asset_path": "/Game/Materials/M_MyMaterial.M_MyMaterial",
  "class": "Material",
  "domain": "Surface",
  "blend_mode": "Masked",
  "shading_model": "DefaultLit",
  "two_sided": true,
  "opacity_mask_clip_value": 0.5,
  "changed_properties": ["domain", "blend_mode", "shading_model", "two_sided", "opacity_mask_clip_value"]
}
```

| Field                     | Type     | Description                                         |
| ------------------------- | -------- | --------------------------------------------------- |
| `name`                    | string   | Material asset name                                 |
| `asset_path`              | string   | Full asset path                                     |
| `class`                   | string   | Always `Material`                                   |
| `domain`                  | string   | Current domain after changes                        |
| `blend_mode`              | string   | Current blend mode after changes                    |
| `shading_model`           | string   | Current shading model after changes                 |
| `two_sided`               | boolean  | Current two-sided state                             |
| `opacity_mask_clip_value` | number   | Current clip value                                  |
| `changed_properties`      | array    | Array of property names that were modified           |

## Examples

### Change blend mode to Masked with clip value

```json
{
  "asset_path": "/Game/Materials/M_Foliage",
  "blend_mode": "Masked",
  "opacity_mask_clip_value": 0.333
}
```

### Enable two-sided rendering

```json
{
  "asset_path": "/Game/Materials/M_Leaf",
  "two_sided": true
}
```

### Change domain and shading model

```json
{
  "asset_path": "/Game/Materials/M_Decal",
  "domain": "DeferredDecal",
  "shading_model": "DefaultLit"
}
```

## Notes

- Only works on base `UMaterial` assets — use `set_material_parameter` to modify material instances
- The material is recompiled and saved to disk after all properties are applied
- All response fields reflect the material state **after** changes are applied
- `opacity_mask_clip_value` is only meaningful when `blend_mode` is `Masked`, but can be set regardless
- The `changed_properties` array lists only properties that were explicitly provided in the request

## Errors

| Condition                          | `isError` | Message                                                        |
| ---------------------------------- | --------- | -------------------------------------------------------------- |
| Missing `asset_path`               | `true`    | `Missing required parameter 'asset_path'`                      |
| Material not found                 | `true`    | `Material not found: '...'`                                    |
| Asset is a material instance       | `true`    | `Asset is a Material Instance, not a base Material: '...'`     |
| Invalid `domain` value             | `true`    | `Invalid domain: '...'`                                        |
| Invalid `blend_mode` value         | `true`    | `Invalid blend_mode: '...'`                                    |
| Invalid `shading_model` value      | `true`    | `Invalid shading_model: '...'`                                 |
| No properties provided             | `true`    | `No properties to set — provide at least one optional parameter` |
