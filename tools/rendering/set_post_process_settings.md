# set_post_process_settings

Set Post Process Volume settings. Override flags are set automatically when a property is modified, ensuring the engine applies the new values.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `volume_name` | string | **Yes** | Actor name or label of the Post Process Volume to modify. |
| `settings` | object | **Yes** | Key/value pairs of settings to apply. Supported keys: `bloom_intensity`, `bloom_threshold`, `bloom_size_scale`, `auto_exposure_method`, `auto_exposure_bias`, `auto_exposure_min_brightness`, `auto_exposure_max_brightness`, `dof_focal_distance`, `dof_focal_region`, `dof_near_blur_size`, `dof_far_blur_size`, `dof_fstop`, `dof_sensor_width`, `motion_blur_amount`, `motion_blur_max`, `motion_blur_target_fps`, `ao_intensity`, `ao_radius`, `ao_power`, `vignette_intensity`, `color_grading_intensity`, `white_temp`, `white_tint`, `lens_flare_intensity`, `lens_flare_bokeh_size`, `lens_flare_threshold`, `gi_method`, `reflection_method`, `priority`, `blend_radius`, `blend_weight`, `enabled`, `unbound`. |

## Example

**Request:**
```json
{
  "tool": "set_post_process_settings",
  "arguments": {
    "volume_name": "PostProcessVolume_0",
    "settings": {
      "bloom_intensity": 1.5,
      "bloom_threshold": 0.8,
      "ao_intensity": 0.75,
      "vignette_intensity": 0.6,
      "auto_exposure_bias": 1.2
    }
  }
}
```

**Response:**
```json
{
  "volume_name": "PostProcessVolume_0",
  "applied": [
    { "key": "bloom_intensity", "value": 1.5 },
    { "key": "bloom_threshold", "value": 0.8 },
    { "key": "ao_intensity", "value": 0.75 },
    { "key": "vignette_intensity", "value": 0.6 },
    { "key": "auto_exposure_bias", "value": 1.2 }
  ],
  "applied_count": 5
}
```

## Notes

- Override flags are set automatically for each modified property so the engine respects the new values.
- Only the specified settings are changed; all other Post Process Volume properties remain untouched.
- Volume-level properties (`priority`, `blend_radius`, `blend_weight`, `enabled`, `unbound`) are set directly on the volume actor, not on the post process settings struct.
- The volume is resolved by matching against both its actor name and display label.
- Returns an error if the specified volume is not found in the current level.
