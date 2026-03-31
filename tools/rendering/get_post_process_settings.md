# get_post_process_settings

Get Post Process Volume settings. Can query a specific volume by name or return all Post Process Volumes in the current level.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `volume_name` | string | No | Actor name or label of the Post Process Volume. If omitted, returns all PPVs in the level. |
| `category` | string | No | Filter category: `all` (default), `bloom`, `exposure`, `dof`, `motion_blur`, `ao`, `vignette`, `color_grading`, `lens_flare`, `gi`, `reflections` |

## Example

**Request (single volume):**
```json
{
  "tool": "get_post_process_settings",
  "arguments": {
    "volume_name": "PostProcessVolume_0",
    "category": "bloom"
  }
}
```

**Response (single volume):**
```json
{
  "volume_name": "PostProcessVolume_0",
  "label": "MainPPV",
  "priority": 0.0,
  "blend_radius": 100.0,
  "blend_weight": 1.0,
  "enabled": true,
  "unbound": true,
  "bloom": {
    "intensity": 0.675,
    "threshold": -1.0,
    "size_scale": 4.0
  }
}
```

**Request (all volumes):**
```json
{
  "tool": "get_post_process_settings",
  "arguments": {}
}
```

**Response (all volumes):**
```json
{
  "post_process_volumes": [
    {
      "name": "PostProcessVolume_0",
      "label": "MainPPV",
      "priority": 0.0,
      "blend_radius": 100.0,
      "blend_weight": 1.0,
      "enabled": true,
      "unbound": true,
      "bloom": { "intensity": 0.675, "threshold": -1.0, "size_scale": 4.0 },
      "exposure": { "method": 1, "bias": 1.0, "min_brightness": 0.03, "max_brightness": 2.0 },
      "depth_of_field": { "focal_distance": 0.0, "focal_region": 0.0, "near_blur_size": 15.0, "far_blur_size": 15.0, "fstop": 4.0, "sensor_width": 24.576 },
      "motion_blur": { "amount": 0.5, "max": 5.0, "target_fps": 30 },
      "ambient_occlusion": { "intensity": 0.5, "radius": 200.0, "power": 2.0 },
      "vignette": { "intensity": 0.4 },
      "color_grading": { "intensity": 1.0, "white_temp": 6500.0, "white_tint": 0.0 },
      "lens_flare": { "intensity": 1.0, "bokeh_size": 3.0, "threshold": 8.0 },
      "global_illumination": { "method": 1 },
      "reflections": { "method": 1 }
    }
  ],
  "count": 1
}
```

## Notes

- When `volume_name` is omitted, all Post Process Volumes in the current level are returned.
- The `category` filter limits which settings sections are included in the response.
- When `category` is `all` or omitted, all sections (bloom, exposure, dof, motion_blur, ao, vignette, color_grading, lens_flare, gi, reflections) are returned.
- Volume properties (`priority`, `blend_radius`, `blend_weight`, `enabled`, `unbound`) are always included regardless of category filter.
