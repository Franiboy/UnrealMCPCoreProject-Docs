# add_static_mesh_socket

**Category:** Mesh  
**Source:** `Private/Mesh/MCPMeshTools_StaticMesh.cpp`

## Description

Add a socket to a Static Mesh asset. Sockets define attachment points for other meshes, effects, or gameplay elements.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | Yes | | Asset path of the Static Mesh |
| `socket_name` | string | Yes | | Name for the new socket |
| `location` | object | No | `{0,0,0}` | Relative location `{x, y, z}` |
| `rotation` | object | No | `{0,0,0}` | Relative rotation `{pitch, yaw, roll}` |
| `scale` | object | No | `{1,1,1}` | Relative scale `{x, y, z}` |
| `tag` | string | No | | Socket tag |

## Response

```json
{
  "socket_name": "GripPoint",
  "total_sockets": 1
}
```

## Examples

### Add a simple socket
```json
{ "tool": "add_static_mesh_socket", "arguments": { "asset_path": "/Game/Meshes/Weapon", "socket_name": "MuzzleFlash" } }
```

### Add a socket with transform
```json
{ "tool": "add_static_mesh_socket", "arguments": { "asset_path": "/Game/Meshes/Weapon", "socket_name": "GripPoint", "location": {"x": 0, "y": 0, "z": 10}, "rotation": {"pitch": 0, "yaw": 90, "roll": 0}, "tag": "Grip" } }
```

## Error Cases

- `asset_path` is required
- `socket_name` is required
- Static Mesh not found at the given path
- Socket already exists with that name
