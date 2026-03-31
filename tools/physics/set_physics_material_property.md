# set_physics_material_property

Set one or more properties on a Physical Material asset.

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `asset_path` | string | Yes | Asset path of the Physical Material. |
| `properties` | object | Yes | Object with property key/value pairs (see below). |

### Supported Properties

#### Surface Properties

| Key | Type | Description |
|-----|------|-------------|
| `friction` | number | Friction coefficient (min: 0) |
| `static_friction` | number | Static friction coefficient (min: 0) |
| `restitution` | number | Restitution / bounciness (clamped 0-1) |
| `friction_combine_mode` | string | How friction is combined: "Average", "Min", "Multiply", "Max" (auto-enables override) |
| `override_friction_combine_mode` | boolean | Whether to override the friction combine mode |
| `restitution_combine_mode` | string | How restitution is combined: "Average", "Min", "Multiply", "Max" (auto-enables override) |
| `override_restitution_combine_mode` | boolean | Whether to override the restitution combine mode |

#### Object Properties

| Key | Type | Description |
|-----|------|-------------|
| `density` | number | Density in g/cm3 (min: 0) |
| `raise_mass_to_power` | number | Mass scaling power (clamped 0.1-1.0) |
| `sleep_linear_velocity_threshold` | number | Linear velocity below which body can sleep (min: 0) |
| `sleep_angular_velocity_threshold` | number | Angular velocity below which body can sleep (min: 0) |
| `sleep_counter_threshold` | number | Frames at low velocity before sleep (min: 0) |

#### Surface Type

| Key | Type | Description |
|-----|------|-------------|
| `surface_type` | string | Surface type name (e.g. "Default", "SurfaceType1", or project-defined name) |

#### Strength

| Key | Type | Description |
|-----|------|-------------|
| `tensile_strength` | number | Tensile strength in MPa |
| `compression_strength` | number | Compression strength in MPa |
| `shear_strength` | number | Shear strength in MPa |

#### Damage

| Key | Type | Description |
|-----|------|-------------|
| `damage_threshold_multiplier` | number | Multiplier for geometry collection damage thresholds |

## Response

Returns a JSON object with:

| Field | Type | Description |
|-------|------|-------------|
| `asset_path` | string | Asset path of the modified Physical Material |
| `properties_set` | number | Number of properties that were set |
| `values` | object | Confirmed values for each property set |

## Example

```json
{
  "method": "tools/call",
  "params": {
    "name": "set_physics_material_property",
    "arguments": {
      "asset_path": "/Game/Physics/PM_Ice",
      "properties": {
        "friction": 0.05,
        "restitution": 0.1,
        "density": 0.92,
        "friction_combine_mode": "Min"
      }
    }
  }
}
```

Response:

```json
{
  "asset_path": "/Game/Physics/PM_Ice.PM_Ice",
  "properties_set": 4,
  "values": {
    "friction": 0.05,
    "restitution": 0.1,
    "density": 0.92,
    "friction_combine_mode": "Min"
  }
}
```

## Errors

| Condition | Error |
|-----------|-------|
| Missing asset_path | "asset_path is required" |
| Asset not found | "Physical Material not found: {path}" |
| Missing properties object | "'properties' object is required" |
| No recognized properties | "No recognized properties in 'properties' object" |
