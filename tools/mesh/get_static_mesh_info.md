# get_static_mesh_info

**Category:** Mesh  
**Source:** `Private/Mesh/MCPMeshTools_StaticMesh.cpp`

## Description

Inspect a Static Mesh asset: LODs (vertex/triangle/section counts per LOD), materials, sockets (location, rotation, scale, tag), bounds, Nanite status, collision settings, and lightmap configuration.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | Yes | | Asset path of the Static Mesh |

## Response

```json
{
  "name": "Shape_Cube",
  "asset_path": "/Game/StarterContent/Shapes/Shape_Cube.Shape_Cube",
  "lod_count": 1,
  "lods": [
    { "index": 0, "vertex_count": 24, "triangle_count": 12, "section_count": 1 }
  ],
  "vertex_count": 24,
  "triangle_count": 12,
  "materials": [
    { "index": 0, "slot_name": "Element0", "material": "/Engine/BasicShapes/BasicShapeMaterial.BasicShapeMaterial" }
  ],
  "material_count": 1,
  "sockets": [
    { "name": "MySocket", "location": {"x":0,"y":0,"z":0}, "rotation": {"pitch":0,"yaw":0,"roll":0}, "scale": {"x":1,"y":1,"z":1}, "tag": "" }
  ],
  "socket_count": 0,
  "bounds": {
    "origin": {"x":0,"y":0,"z":0},
    "extent": {"x":50,"y":50,"z":50},
    "sphere_radius": 86.6
  },
  "nanite_enabled": false,
  "collision": {
    "complexity": "default",
    "box_count": 1,
    "sphere_count": 0,
    "capsule_count": 0,
    "convex_count": 0
  },
  "lightmap_resolution": 64,
  "lightmap_coordinate_index": 1
}
```

## Examples

### Inspect a Static Mesh
```json
{ "tool": "get_static_mesh_info", "arguments": { "asset_path": "/Game/StarterContent/Shapes/Shape_Cube" } }
```

## Error Cases

- `asset_path` is required
- Static Mesh not found at the given path
