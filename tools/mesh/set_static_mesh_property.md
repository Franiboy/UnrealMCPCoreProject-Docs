# set_static_mesh_property

**Category:** Mesh  
**Source:** `Private/Mesh/MCPMeshTools_StaticMesh.cpp`

## Description

Set properties on a Static Mesh asset. Supported properties: `lightmap_resolution` (int), `lightmap_coordinate_index` (int), `nanite_enabled` (bool), `lod_group` (string), `min_lod` (int).

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | Yes | | Asset path of the Static Mesh |
| `properties` | object | Yes | | Properties to set (see supported keys below) |

### Supported Properties

| Key | Type | Description |
|-----|------|-------------|
| `lightmap_resolution` | int | Lightmap resolution (e.g. 32, 64, 128) |
| `lightmap_coordinate_index` | int | UV channel index for lightmaps |
| `nanite_enabled` | bool | Enable/disable Nanite |
| `lod_group` | string | LOD group name (e.g. LevelArchitecture, SmallProp, LargeProp) |
| `min_lod` | int | Minimum LOD index to use |

## Response

```json
{
  "success": true,
  "updated_properties": ["lightmap_resolution", "nanite_enabled"]
}
```

## Examples

### Set lightmap resolution
```json
{ "tool": "set_static_mesh_property", "arguments": { "asset_path": "/Game/StarterContent/Shapes/Shape_Cube", "properties": { "lightmap_resolution": 128 } } }
```

### Enable Nanite
```json
{ "tool": "set_static_mesh_property", "arguments": { "asset_path": "/Game/MyMesh", "properties": { "nanite_enabled": true } } }
```

## Error Cases

- `asset_path` is required
- `properties` is required
- Static Mesh not found at the given path
- No recognized properties provided
