# set_skeletal_mesh_material

**Category:** Mesh  
**Source:** `Private/Mesh/MCPMeshTools_SkeletalMesh.cpp`

## Description

Assign a material to a Skeletal Mesh material slot by index.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | Yes | | Asset path of the Skeletal Mesh |
| `slot_index` | integer | Yes | | 0-based material slot index |
| `material_path` | string | Yes | | Asset path of the material to assign |

## Response

```json
{
  "success": true,
  "slot_index": 0,
  "material": "/Game/Materials/M_Body.M_Body",
  "slot_name": "Body"
}
```

## Examples

### Assign a material to slot 0
```json
{ "tool": "set_skeletal_mesh_material", "arguments": { "asset_path": "/Game/Characters/SK_Mannequin", "slot_index": 0, "material_path": "/Game/Materials/M_Body" } }
```

## Error Cases

- `asset_path` is required
- `slot_index` is required
- `material_path` is required
- Skeletal Mesh not found at the given path
- Invalid slot index (out of range)
- Material not found at the given path
