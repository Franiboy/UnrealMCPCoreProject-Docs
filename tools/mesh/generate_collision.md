# generate_collision

**Category:** Mesh  
**Source:** `Private/Mesh/MCPMeshTools_Operations.cpp`

## Description

Auto-generate collision for a Static Mesh. Supports box, sphere, capsule, and convex decomposition collision types.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | Yes | | Asset path of the Static Mesh |
| `type` | string | Yes | | Collision type: `box`, `sphere`, `capsule`, `convex` |
| `hull_count` | integer | No | `4` | Max convex hulls (convex only) |
| `hull_precision` | integer | No | `100000` | Hull precision/voxels (convex only) |
| `max_hull_verts` | integer | No | `16` | Max vertices per hull (convex only) |

## Response

```json
{
  "success": true,
  "asset_path": "/Game/StarterContent/Shapes/Shape_Cube.Shape_Cube",
  "collision_type": "box",
  "primitive_count": 1,
  "box_count": 1,
  "sphere_count": 0,
  "capsule_count": 0,
  "convex_count": 0
}
```

## Examples

### Generate box collision
```json
{ "tool": "generate_collision", "arguments": { "asset_path": "/Game/StarterContent/Shapes/Shape_Cube", "type": "box" } }
```

### Generate convex decomposition
```json
{ "tool": "generate_collision", "arguments": { "asset_path": "/Game/Meshes/ComplexMesh", "type": "convex", "hull_count": 8, "hull_precision": 200000, "max_hull_verts": 32 } }
```

### Generate sphere collision
```json
{ "tool": "generate_collision", "arguments": { "asset_path": "/Game/StarterContent/Shapes/Shape_Sphere", "type": "sphere" } }
```

## Error Cases

- `asset_path` is required
- `type` is required (box, sphere, capsule, convex)
- Static Mesh not found at the given path
- Unknown collision type
- Convex decomposition failed
- StaticMeshEditorSubsystem not available (convex only)
