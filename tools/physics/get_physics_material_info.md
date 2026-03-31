# get_physics_material_info

Inspect a Physical Material asset: friction, restitution, density, surface type, combine modes, sleep thresholds, strength, and damage modifier.

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `asset_path` | string | Yes | Asset path of the Physical Material. |

## Response

Returns a JSON object with:

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Asset name |
| `asset_path` | string | Full asset path |
| `class` | string | Class name |
| `friction` | number | Friction coefficient (default: 0.7) |
| `static_friction` | number | Static friction coefficient (default: 0.0) |
| `friction_combine_mode` | string | How friction is combined: "Average", "Min", "Multiply", "Max" |
| `override_friction_combine_mode` | boolean | Whether friction combine mode is overridden |
| `restitution` | number | Restitution / bounciness (0-1, default: 0.3) |
| `restitution_combine_mode` | string | How restitution is combined |
| `override_restitution_combine_mode` | boolean | Whether restitution combine mode is overridden |
| `density` | number | Density in g/cm3 (default: 1.0) |
| `raise_mass_to_power` | number | Mass scaling power (0.1-1.0, default: 0.75) |
| `sleep_linear_velocity_threshold` | number | Linear velocity below which body can sleep |
| `sleep_angular_velocity_threshold` | number | Angular velocity below which body can sleep |
| `sleep_counter_threshold` | number | Frames at low velocity before sleep |
| `surface_type` | string | Surface type name |
| `surface_type_index` | number | Numeric surface type index (0-62) |
| `strength` | object | Material strength properties (see below) |
| `damage_modifier` | object | Damage modifier properties (see below) |

### strength object

| Field | Type | Description |
|-------|------|-------------|
| `tensile_strength` | number | Tensile strength in MPa (default: 2.0) |
| `compression_strength` | number | Compression strength in MPa (default: 20.0) |
| `shear_strength` | number | Shear strength in MPa (default: 6.0) |

### damage_modifier object

| Field | Type | Description |
|-------|------|-------------|
| `damage_threshold_multiplier` | number | Multiplier for geometry collection damage thresholds (default: 1.0) |

## Example

```json
{
  "method": "tools/call",
  "params": {
    "name": "get_physics_material_info",
    "arguments": {
      "asset_path": "/Game/Physics/PM_Ice"
    }
  }
}
```

Response:

```json
{
  "name": "PM_Ice",
  "asset_path": "/Game/Physics/PM_Ice.PM_Ice",
  "class": "PhysicalMaterial",
  "friction": 0.1,
  "static_friction": 0.0,
  "friction_combine_mode": "Average",
  "override_friction_combine_mode": false,
  "restitution": 0.2,
  "restitution_combine_mode": "Average",
  "override_restitution_combine_mode": false,
  "density": 0.92,
  "raise_mass_to_power": 0.75,
  "sleep_linear_velocity_threshold": 1.0,
  "sleep_angular_velocity_threshold": 0.05,
  "sleep_counter_threshold": 4,
  "surface_type": "Default",
  "surface_type_index": 0,
  "strength": {
    "tensile_strength": 2.0,
    "compression_strength": 20.0,
    "shear_strength": 6.0
  },
  "damage_modifier": {
    "damage_threshold_multiplier": 1.0
  }
}
```

## Errors

| Condition | Error |
|-----------|-------|
| Missing asset_path | "asset_path is required" |
| Asset not found | "Physical Material not found: {path}" |
