# set_static_mesh_material

**Category:** Mesh  
**Source:** `Private/Mesh/MCPMeshTools_StaticMesh.cpp`

## Description

Assign a material to a Static Mesh material slot by index.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | Yes | | Asset path of the Static Mesh |
| `slot_index` | integer | Yes | | 0-based material slot index |
| `material_path` | string | Yes | | Asset path of the material to assign |

## Response

```json
{
  "success": true,
  "slot_index": 0,
  "material": "/Game/Materials/M_MyMaterial.M_MyMaterial",
  "slot_name": "Element0"
}
```

## Examples

### Assign a material to slot 0
```json
{ "tool": "set_static_mesh_material", "arguments": { "asset_path": "/Game/StarterContent/Shapes/Shape_Cube", "slot_index": 0, "material_path": "/Game/StarterContent/Materials/M_Basic_Floor" } }
```

## Error Cases

- `asset_path` is required
- `slot_index` is required
- `material_path` is required
- Static Mesh not found at the given path
- Invalid slot index (out of range)
- Material not found at the given path
