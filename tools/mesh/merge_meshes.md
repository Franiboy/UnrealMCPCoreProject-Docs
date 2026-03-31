# merge_meshes

**Category:** Mesh  
**Source:** `Private/Mesh/MCPMeshTools_Operations.cpp`

## Description

Merge multiple Static Mesh actors in the current level into a single Static Mesh asset. Uses UE5's IMeshMergeUtilities.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `actor_names` | array of strings | Yes | | Array of actor names/labels to merge (at least 2) |
| `output_name` | string | Yes | | Name for the output merged mesh asset |
| `output_path` | string | No | `/Game` | Output content path |
| `merge_materials` | boolean | No | `false` | Merge materials into one atlas |
| `merge_physics` | boolean | No | `true` | Merge physics/collision data |

## Response

```json
{
  "name": "SM_MergedMesh",
  "asset_path": "/Game/SM_MergedMesh.SM_MergedMesh",
  "lod_count": 1,
  "vertex_count": 48,
  "triangle_count": 24,
  "material_count": 2,
  "merged_actors": 2
}
```

## Examples

### Merge two actors
```json
{ "tool": "merge_meshes", "arguments": { "actor_names": ["Cube1", "Cube2"], "output_name": "SM_MergedCubes" } }
```

### Merge with material merging
```json
{ "tool": "merge_meshes", "arguments": { "actor_names": ["Wall1", "Wall2", "Wall3"], "output_name": "SM_MergedWalls", "output_path": "/Game/Merged", "merge_materials": true } }
```

## Error Cases

- `actor_names` is required (array of at least 2 actor names)
- `output_name` is required
- Actor not found in the current level
- Need at least 2 static mesh components to merge
- No editor world available
- Merge operation did not produce a Static Mesh
