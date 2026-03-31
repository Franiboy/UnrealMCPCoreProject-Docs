# remove_static_mesh_socket

**Category:** Mesh  
**Source:** `Private/Mesh/MCPMeshTools_StaticMesh.cpp`

## Description

Remove a socket from a Static Mesh asset by name.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | Yes | | Asset path of the Static Mesh |
| `socket_name` | string | Yes | | Name of the socket to remove |

## Response

```json
{
  "success": true,
  "removed_socket": "MuzzleFlash",
  "remaining_sockets": 0
}
```

## Examples

### Remove a socket
```json
{ "tool": "remove_static_mesh_socket", "arguments": { "asset_path": "/Game/Meshes/Weapon", "socket_name": "MuzzleFlash" } }
```

## Error Cases

- `asset_path` is required
- `socket_name` is required
- Static Mesh not found at the given path
- Socket not found with that name
