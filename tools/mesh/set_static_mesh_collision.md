# set_static_mesh_collision

**Category:** Mesh  
**Source:** `Private/Mesh/MCPMeshTools_StaticMesh.cpp`

## Description

Configure collision settings on a Static Mesh: collision complexity and which LOD to use for collision.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | Yes | | Asset path of the Static Mesh |
| `complexity` | string | No | | Collision complexity: `default`, `simple_and_complex`, `use_simple`, `use_complex` |
| `lod_for_collision` | integer | No | | LOD index to use for collision mesh generation |

At least one of `complexity` or `lod_for_collision` must be provided.

## Response

```json
{
  "success": true,
  "complexity": "use_complex",
  "lod_for_collision": 0
}
```

## Examples

### Set collision complexity
```json
{ "tool": "set_static_mesh_collision", "arguments": { "asset_path": "/Game/StarterContent/Shapes/Shape_Cube", "complexity": "use_complex" } }
```

### Set LOD for collision
```json
{ "tool": "set_static_mesh_collision", "arguments": { "asset_path": "/Game/MyMesh", "lod_for_collision": 1 } }
```

## Error Cases

- `asset_path` is required
- Static Mesh not found at the given path
- Unknown complexity value
- No properties specified (need at least `complexity` or `lod_for_collision`)
