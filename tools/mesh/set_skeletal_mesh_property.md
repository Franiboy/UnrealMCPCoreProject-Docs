# set_skeletal_mesh_property

**Category:** Mesh  
**Source:** `Private/Mesh/MCPMeshTools_SkeletalMesh.cpp`

## Description

Set properties on a Skeletal Mesh asset. Supported properties: `physics_asset` (string path), `min_lod` (int).

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | Yes | | Asset path of the Skeletal Mesh |
| `properties` | object | Yes | | Properties to set (see supported keys below) |

### Supported Properties

| Key | Type | Description |
|-----|------|-------------|
| `physics_asset` | string | Asset path of the Physics Asset to assign |
| `min_lod` | int | Minimum LOD index to use |

## Response

```json
{
  "success": true,
  "updated_properties": ["physics_asset"]
}
```

## Examples

### Set physics asset
```json
{ "tool": "set_skeletal_mesh_property", "arguments": { "asset_path": "/Game/Characters/SK_Mannequin", "properties": { "physics_asset": "/Game/Characters/PA_Mannequin" } } }
```

### Set minimum LOD
```json
{ "tool": "set_skeletal_mesh_property", "arguments": { "asset_path": "/Game/Characters/SK_Mannequin", "properties": { "min_lod": 1 } } }
```

## Error Cases

- `asset_path` is required
- `properties` is required
- Skeletal Mesh not found at the given path
- Physics Asset not found at the given path
- No recognized properties provided
