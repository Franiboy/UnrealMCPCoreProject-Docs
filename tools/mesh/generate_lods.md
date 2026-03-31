# generate_lods

**Category:** Mesh  
**Source:** `Private/Mesh/MCPMeshTools_Operations.cpp`

## Description

Auto-generate LODs for a Static Mesh or Skeletal Mesh using its LOD group settings. For Static Meshes, generates LODs in-package using the engine's LOD generation pipeline. For Skeletal Meshes, reports current LODs (full generation requires the mesh reduction plugin).

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | Yes | | Asset path of the Static Mesh or Skeletal Mesh |
| `lod_group` | string | No | | LOD group to set before generating (e.g. `LevelArchitecture`, `SmallProp`, `LargeProp`, `Foliage`, `HighDetail`) |

## Response (Static Mesh)

```json
{
  "name": "Shape_Cube",
  "asset_path": "/Game/StarterContent/Shapes/Shape_Cube.Shape_Cube",
  "type": "StaticMesh",
  "lod_count": 3,
  "lods": [
    { "index": 0, "vertex_count": 24, "triangle_count": 12 },
    { "index": 1, "vertex_count": 16, "triangle_count": 8 },
    { "index": 2, "vertex_count": 8, "triangle_count": 4 }
  ]
}
```

## Response (Skeletal Mesh)

```json
{
  "name": "SK_Mannequin",
  "asset_path": "/Game/Characters/SK_Mannequin.SK_Mannequin",
  "type": "SkeletalMesh",
  "lod_count": 1,
  "note": "Skeletal mesh LOD generation requires the mesh reduction plugin. Use the editor UI for full LOD generation."
}
```

## Examples

### Generate LODs for a Static Mesh
```json
{ "tool": "generate_lods", "arguments": { "asset_path": "/Game/StarterContent/Shapes/Shape_Cube" } }
```

### Generate LODs with a specific LOD group
```json
{ "tool": "generate_lods", "arguments": { "asset_path": "/Game/Environment/SM_Building", "lod_group": "LevelArchitecture" } }
```

## Error Cases

- `asset_path` is required
- Mesh not found at the given path (neither Static Mesh nor Skeletal Mesh)
