# get_skeletal_mesh_info

**Category:** Mesh  
**Source:** `Private/Mesh/MCPMeshTools_SkeletalMesh.cpp`

## Description

Inspect a Skeletal Mesh asset: LODs, materials, skeleton (bone count), physics asset, morph targets, bounds, sockets, and Nanite status.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | Yes | | Asset path of the Skeletal Mesh |

## Response

```json
{
  "name": "SK_Mannequin",
  "asset_path": "/Game/Characters/SK_Mannequin.SK_Mannequin",
  "lod_count": 2,
  "lods": [
    { "index": 0, "screen_size": 1.0 },
    { "index": 1, "screen_size": 0.5 }
  ],
  "materials": [
    { "index": 0, "slot_name": "Body", "material": "/Game/Materials/M_Body.M_Body" }
  ],
  "material_count": 1,
  "skeleton": "/Game/Characters/SK_Mannequin_Skeleton.SK_Mannequin_Skeleton",
  "bone_count": 67,
  "physics_asset": "/Game/Characters/SK_Mannequin_PhysicsAsset.SK_Mannequin_PhysicsAsset",
  "morph_targets": ["Smile", "Frown"],
  "morph_target_count": 2,
  "bounds": {
    "origin": {"x":0,"y":0,"z":90},
    "extent": {"x":30,"y":30,"z":90},
    "sphere_radius": 95.0
  },
  "sockets": [
    { "name": "hand_r", "bone_name": "hand_r", "location": {"x":0,"y":0,"z":0}, "rotation": {"pitch":0,"yaw":0,"roll":0}, "scale": {"x":1,"y":1,"z":1} }
  ],
  "socket_count": 1,
  "nanite_enabled": false
}
```

## Examples

### Inspect a Skeletal Mesh
```json
{ "tool": "get_skeletal_mesh_info", "arguments": { "asset_path": "/Game/Characters/SK_Mannequin" } }
```

## Error Cases

- `asset_path` is required
- Skeletal Mesh not found at the given path
