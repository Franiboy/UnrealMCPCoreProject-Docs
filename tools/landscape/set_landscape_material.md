# set_landscape_material

Assign a material (and optionally a hole material) to a Landscape actor.

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `material` | string | No* | Asset path of the material to assign (e.g. `/Game/Materials/M_Landscape`). |
| `hole_material` | string | No* | Asset path of the hole material, or `"None"` to clear. |
| `actor_label` | string | No | Label or name of the Landscape actor. If omitted, uses the first Landscape found. |

*At least one of `material` or `hole_material` must be provided.

## Response

Returns a JSON object with:

| Field | Type | Description |
|-------|------|-------------|
| `actor_label` | string | Display label of the Landscape actor |
| `landscape_material` | string | New landscape material path (or "None") |
| `landscape_hole_material` | string | New hole material path (or "None") |
| `previous_material` | string | Previous landscape material path |
| `previous_hole_material` | string | Previous hole material path |

## Example

```json
{
  "method": "tools/call",
  "params": {
    "name": "set_landscape_material",
    "arguments": {
      "material": "/Game/Materials/M_Landscape"
    }
  }
}
```

Response:

```json
{
  "actor_label": "Landscape",
  "landscape_material": "/Game/Materials/M_Landscape.M_Landscape",
  "landscape_hole_material": "None",
  "previous_material": "None",
  "previous_hole_material": "None"
}
```

## Errors

| Condition | Error |
|-----------|-------|
| No editor world | "No editor world available" |
| No landscape in level | "No Landscape actor found in the current level" |
| Actor label not found | "No Landscape actor found matching '{label}'" |
| Material not found | "Material not found: {path}" |
| Hole material not found | "Hole material not found: {path}" |
| No material specified | "No material or hole_material specified" |
