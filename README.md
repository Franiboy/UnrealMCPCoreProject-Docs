# UnrealMCPCore

A pure C++ Unreal Engine 5 Editor plugin that exposes an MCP (Model Context Protocol) server over HTTP (default) or HTTPS, enabling AI clients like Devin to read, write, and interact with Blueprints, Assets, Levels, and more, all without Python or Node.js dependencies.

## Overview

UnrealMCPCore runs an HTTP server inside the Unreal Editor on `localhost:8090`, speaking JSON-RPC 2.0 over the MCP protocol. No manual setup required. Just open the project and connect. For production or remote use, enable HTTPS (TLS 1.2+) in settings; a self-signed certificate is generated automatically. AI agents connect to it and use registered tools to inspect and manipulate the project.

**Engine Version:** UE 5.5  
**Platform:** Win64, Mac, Linux  
**Module Type:** Editor  
**Loading Phase:** PostEngineInit

## Quick Start

1. Open the project in Unreal Editor. The MCP server starts automatically on port 8090
2. Verify with: `http://localhost:8090/mcp/health`
3. Connect your MCP client to `http://localhost:8090/mcp`

### Console Commands

| Command            | Description                                                     |
| ------------------ | --------------------------------------------------------------- |
| `MCP.Start [Port]` | Start the MCP server (default port from settings)               |
| `MCP.Stop`         | Stop the MCP server                                             |
| `MCP.Status`       | Show server status, active session, request stats, and settings |

### Settings

Accessible via **Project Settings > Plugins > MCP Core**.

| Setting               | Default | Description                                                                                          |
| --------------------- | ------- | ---------------------------------------------------------------------------------------------------- |
| **Server Port**       | `8090`  | The port the server listens on. Requires editor restart.                                             |
| **Enable HTTPS**      | `false` | Enable HTTPS (TLS 1.2+) with auto-generated self-signed certificates. When disabled (default), uses plain HTTP for zero-config setup. Requires editor restart. |
| **Auto Start Server** | `true`  | Automatically start the server when the editor loads.                                                |
| **Verbose Logging**   | `false` | Log all MCP requests and responses in detail. Toggles live, no restart needed.                      |
| **Max Log Entries**   | `1000`  | Maximum request log entries kept in memory.                                                          |
| **Enable CORS**       | `true`  | Add CORS headers to responses (required for browser-based clients).                                  |
| **CORS Origin**       | `*`     | Allowed origin for CORS (`*` = any).                                                                 |
| **Require API Key**   | `false` | Require an API key for all MCP requests. Clients must send `X-API-Key` or `Authorization: Bearer <key>`. The `/mcp/health` endpoint is always accessible without authentication. |
| **API Key**           | *(empty)* | The API key clients must provide. Displayed as a password field. Leave empty to disable auth regardless of the toggle. |
| **Enable Rate Limiting** | `false` | Throttle requests with a per-session token bucket. Returns HTTP 429 when exceeded.                |
| **Max Requests/Min**  | `60`    | Sustained token refill rate (tokens per minute). Only applies when rate limiting is enabled.          |
| **Burst Size**        | `10`    | Maximum burst of requests allowed before throttling starts. Only applies when rate limiting is enabled.|
| **Tool Timeout (s)**  | `30`    | Maximum tool execution time in seconds. Exceeded tools are flagged with `_timeout_exceeded`. Set to 0 to disable. |

Settings are stored in `Config/DefaultMCPCore.ini` and can be overridden per-platform.

### MCP Monitor Panel

The MCP Monitor is a **status bar drawer**, a button in the bottom status bar (next to Output Log and Content Drawer) that slides up a monitoring panel when clicked.

- **Level Editor**: The "MCP Monitor" button appears automatically in the bottom status bar ~2 seconds after the editor starts.
- **Blueprint / Asset Editors**: The drawer is registered automatically when any asset editor opens.
- **Dock in Layout**: Click the "Dock in Layout" button (top-right of the panel) to open the MCP Monitor as a permanent docked tab. When clicked from an asset editor (Blueprint, Material, etc.) it docks in that editor; from the Level Editor it docks in the main layout. The drawer remains usable alongside the docked tab. The button is hidden when the panel is already docked.

**Features:**

| Section       | Description                                                                                      |
| ------------- | ------------------------------------------------------------------------------------------------ |
| **Header**    | Single row: live server status, session count, tool count, and request stats (`Server: Running on port 8090 | No session | 26 tools | Requests: 3 (2 OK, 1 FAIL)`) |
| **Controls**  | Start/Stop Server, Restart, Clear Log, Client Config, Export, Report Issue, Dock in Layout buttons |
| **Log Table** | Scrollable table of all MCP tool calls with columns: #, Tool, Status, Details. Supports multi-select (Ctrl+Click, Shift+Click). Horizontal/vertical split toggle (list + detail side by side or stacked). |
| **Copy**      | Copy selected rows via right-click context menu or Ctrl+C. Multiple rows are copied as newline-separated text sorted by ID. |
| **Revert**    | Right-click successful mutating entries (`create_blueprint`, `set_blueprint_defaults`, etc.) and select "Revert" to undo. Supports multi-select (newest first). Reverted entries show status "REV" in grey. |
| **Retry**     | Right-click FAIL or REV entries and select "Retry" to re-execute the tool call with the original arguments. Supports multi-select (oldest first). Each retry creates a new log entry with full revert support. |
| **Details**   | Time, duration, summary of tool arguments with clickable asset hyperlink, per-request log messages. On failure: error message in orange. |
| **Auto-Scroll** | Automatically scrolls to the latest entry when at the bottom of the list. Scroll up to pause, scroll back to the bottom to resume. |

The panel refreshes automatically every 0.5 seconds. Only tool calls are shown in the log. Protocol methods (`initialize`, `ping`, `tools/list`, `resources/list`) are handled silently.

### Client Config Helper

Click the **"Client Config"** button (available in the Monitor Panel header and in Project Settings > MCP Core) to open a popup dialog with the ready-to-paste JSON snippet for your MCP client. A transport dropdown lets you switch between **HTTP** and **stdio**. The snippet updates automatically with the current server port, the correct URL scheme (HTTP or HTTPS depending on the Enable HTTPS setting), and the resolved path to the stdio proxy script.

## MCP Protocol

The server implements the [Model Context Protocol](https://modelcontextprotocol.io/) over Streamable HTTP transport:

- **Endpoint:** `POST http://localhost:8090/mcp` (JSON-RPC 2.0)
- **Discovery:** `GET http://localhost:8090/mcp` (returns SSE `event: endpoint`)
- **Health:** `GET http://localhost:8090/mcp/health`
- **Session Termination:** `DELETE http://localhost:8090/mcp`

### SSE (Server-Sent Events) Support

The server supports the **Streamable HTTP** transport defined by the MCP spec. Clients that send `Accept: text/event-stream` receive responses wrapped in SSE format (`event: message`, `data: {…}`). Clients that only send `Accept: application/json` (or no Accept header) receive plain JSON-RPC responses as before. Full backward compatibility is preserved.

`GET /mcp` returns an SSE `event: endpoint` with `data: {"uri":"…"}` for transport discovery, as specified by the Streamable HTTP protocol.

> **Note:** The server uses a custom TCP/TLS server (OpenSSL when HTTPS is enabled) with one-shot request/response semantics, so each SSE response is a single completed event, not a persistent stream. This covers all current MCP tool call semantics and lays the groundwork for future true-streaming support.

### stdio Transport (Proxy)

For MCP clients that only support **stdio** transport (spawning the server as a subprocess), a lightweight Python proxy script is included. It reads JSON-RPC from stdin, forwards to the HTTP server (or HTTPS with `--https` flag), and writes responses to stdout. No external dependencies. Python 3.7+ stdlib only.

**Claude Desktop** (`claude_desktop_config.json`):
```json
{
    "mcpServers": {
        "unreal": {
            "command": "python",
            "args": ["C:/path/to/Plugins/UnrealMCPCore/scripts/unrealmcpcore_stdio.py"]
        }
    }
}
```

**Custom host/port:**
```json
{
    "mcpServers": {
        "unreal": {
            "command": "python",
            "args": [
                "C:/path/to/Plugins/UnrealMCPCore/scripts/unrealmcpcore_stdio.py",
                "--host", "192.168.1.100",
                "--port", "9090"
            ]
        }
    }
}
```

### Supported Methods

| Method | Description |
| --- | --- |
| `initialize` | MCP handshake. Creates or reuses a session |
| `notifications/initialized` | Client confirms initialization |
| `ping` | Health check (returns `{"status":"ok"}`) |
| `tools/list` | List all registered tools with input schemas |
| `tools/call` | Execute a tool by name with arguments |
| `resources/list` | Returns `{"resources":[]}`. No resources exposed, but handled cleanly for MCP client compatibility |

### Session & Auto-Initialization

The server supports **multiple concurrent sessions**. Several AI agents (Devin, Claude Desktop, Cursor, etc.) can connect simultaneously, each with its own session. All shared state (sessions, tool registry, rate-limit buckets) is protected by `FCriticalSection` locks, so concurrent requests are safe. Tool execution itself runs outside any lock to avoid blocking other requests.

- **`initialize`** creates a new session and returns a unique session ID in the `Mcp-Session-Id` response header. Clients must include this header in subsequent requests.
- **Re-initialization** is idempotent. Calling `initialize` with an existing session ID reuses that session.
- **Auto-initialization**: if a client sends `tools/call` or `tools/list` without a prior handshake, the server automatically creates a session and processes the request. This means clients do not need to detect editor restarts or re-initialize manually.
- **`DELETE /mcp`** with a `Mcp-Session-Id` header terminates that specific session.
- The health endpoint reports the current session count. The MCP Monitor panel and `MCP.Status` console command show all active sessions.

### Rate Limiting

When **Enable Rate Limiting** is turned on in settings, the server applies a **token-bucket** throttle per session (falling back to peer IP when no session header is present).

- Each client starts with **Burst Size** tokens (default 10).
- Tokens refill at **Max Requests Per Minute / 60** per second (default 1/s).
- Each `POST /mcp` request consumes one token. When the bucket is empty the server responds with **HTTP 429 Too Many Requests** and a `Retry-After` header indicating how many seconds to wait.
- The health endpoint (`GET /mcp/health`) is never rate-limited and reports the current rate-limit configuration in its JSON response.

Rate limiting is **disabled by default** so existing workflows are unaffected. Enable it to protect the editor from runaway loops or excessive automation.

### API Key Authentication

When **Require API Key** is enabled in settings and an **API Key** is configured, every request to `/mcp` (POST, GET, DELETE) must include the key in one of two ways:

- **Header:** `X-API-Key: <your-key>`
- **Header:** `Authorization: Bearer <your-key>`

Missing or invalid keys receive **HTTP 401** or **HTTP 403** respectively. The `/mcp/health` endpoint is always accessible without authentication.

The stdio proxy supports the `--api-key` argument to forward the key automatically:

```bash
python unrealmcpcore_stdio.py --api-key my-secret-key
```

Authentication is **disabled by default**. Enable it when exposing the server to a network or when multiple users share a machine.

## Tools

360 tools available (46 Blueprint + 14 Asset + 14 Level + 17 Material + 9 DataTable/Struct + 26 Widget/UI + 28 Animation + 13 PCG + 11 Niagara + 13 Sequencer + 7 Behavior Tree + 6 Blackboard + 7 State Tree + 5 EQS + 5 Smart Object + 12 Mesh + 8 Enhanced Input + 13 GAS + 19 Audio + 9 Landscape + 12 Physics + 7 Foliage + 7 World Partition + 6 Control Rig + 6 Rendering Config + 6 Curve Asset + 5 Motion Design + 18 Project + 1 Infrastructure + 10 PIE / Testing). Full input/output documentation with examples: **[docs/tools/](tools/README.md)**

#### Blueprint Read Tools (8)

| Tool | Description |
| ---- | ----------- |
| [`get_blueprint`](tools/blueprint/get_blueprint.md) | Full Blueprint introspection (variables, functions, components, graphs, class defaults) with category filtering |
| [`list_blueprints`](tools/blueprint/list_blueprints.md) | List/search Blueprints via Asset Registry with path, type, and name filters |
| [`compile_blueprint`](tools/blueprint/compile_blueprint.md) | Single or batch compilation with error/warning collection |
| [`get_blueprint_graph`](tools/blueprint/get_blueprint_graph.md) | Node-level graph data: pins, connections, positions, types |
| [`get_blueprint_node`](tools/blueprint/get_blueprint_node.md) | Single-node deep inspection with reflection-based properties bag |
| [`search_blueprint_nodes`](tools/blueprint/search_blueprint_nodes.md) | Search nodes by title, class, or comment across all graphs |
| [`get_blueprint_diff`](tools/blueprint/get_blueprint_diff.md) | Compare two Blueprints structurally (variables, functions, components, graphs, defaults) |
| [`search_blueprint_defaults`](tools/blueprint/search_blueprint_defaults.md) | Search class default property values across multiple Blueprints by name, type, or value substring |

#### Blueprint Write Tools (21)

| Tool | Description |
| ---- | ----------- |
| [`create_blueprint`](tools/blueprint/create_blueprint.md) | Create a Blueprint with configurable parent class (C++ name, full path, or BP class) |
| [`delete_blueprint`](tools/blueprint/delete_blueprint.md) | Delete with reference checking and optional force mode |
| [`rename_blueprint`](tools/blueprint/rename_blueprint.md) | Rename/move a Blueprint with automatic redirector creation |
| [`duplicate_blueprint`](tools/blueprint/duplicate_blueprint.md) | Copy a Blueprint to a new name/path, preserving parent class |
| [`reparent_blueprint`](tools/blueprint/reparent_blueprint.md) | Change parent class, recompile, return old/new parent info |
| [`add_blueprint_variable`](tools/blueprint/add_blueprint_variable.md) | Add a typed member variable with metadata (category, default, tooltip, visibility flags) |
| [`remove_blueprint_variable`](tools/blueprint/remove_blueprint_variable.md) | Remove a member variable by name, returns removed type and metadata for undo |
| [`set_blueprint_variable_properties`](tools/blueprint/set_blueprint_variable_properties.md) | Change variable type, default, category, tooltip, flags, replication, transient, save-game |
| [`add_blueprint_function`](tools/blueprint/add_blueprint_function.md) | Create a function graph with typed inputs/outputs, return type, pure/const/access flags, category, tooltip |
| [`implement_function_override`](tools/blueprint/implement_function_override.md) | Override a BlueprintImplementableEvent / BlueprintNativeEvent from a parent class with correct signature |
| [`add_blueprint_macro`](tools/blueprint/add_blueprint_macro.md) | Create a macro graph with optional tunnel pins (supports exec pins for multi-output flow control) |
| [`remove_blueprint_function`](tools/blueprint/remove_blueprint_function.md) | Remove a function graph by name, returns full signature for undo |
| [`remove_blueprint_macro`](tools/blueprint/remove_blueprint_macro.md) | Remove a macro graph by name, returns tunnel pin metadata for undo |
| [`add_blueprint_component`](tools/blueprint/add_blueprint_component.md) | Add a component to the Blueprint's component tree with optional transform and parent attachment |
| [`remove_blueprint_component`](tools/blueprint/remove_blueprint_component.md) | Remove a component by name, returns class/parent/transform for undo |
| [`set_component_property`](tools/blueprint/set_component_property.md) | Set properties on a component template with nested dot-notation paths |
| [`add_blueprint_interface`](tools/blueprint/add_blueprint_interface.md) | Implement an interface on a Blueprint (C++ or Blueprint Interface) |
| [`remove_blueprint_interface`](tools/blueprint/remove_blueprint_interface.md) | Remove an interface from a Blueprint, deleting associated function graphs |
| [`add_event_dispatcher`](tools/blueprint/add_event_dispatcher.md) | Add a multicast delegate (event dispatcher) with optional typed parameters |
| [`remove_event_dispatcher`](tools/blueprint/remove_event_dispatcher.md) | Remove an event dispatcher by name, returns parameters for undo |
| [`set_blueprint_defaults`](tools/blueprint/set_blueprint_defaults.md) | Modify CDO properties with nested dot-notation paths (e.g. `MyVector.X`) |

#### Graph / Node Tools (17)

| Tool | Description |
| ---- | ----------- |
| [`add_node`](tools/blueprint/add_node.md) | Add a node to a graph by K2Node class (function calls, custom events, variable get/set, flow control) |
| [`remove_node`](tools/blueprint/remove_node.md) | Remove a node from a graph by node ID |
| [`connect_pins`](tools/blueprint/connect_pins.md) | Wire two pins together (validates type compatibility via graph schema) |
| [`disconnect_pins`](tools/blueprint/disconnect_pins.md) | Break a wire between two pins |
| [`set_pin_default`](tools/blueprint/set_pin_default.md) | Set the default value on an input pin (string, bool, float, enum, struct literals) |
| [`move_node`](tools/blueprint/move_node.md) | Reposition a node in the graph editor (cosmetic, no recompile) |
| [`add_comment`](tools/blueprint/add_comment.md) | Add a comment box to a Blueprint graph |
| [`add_custom_event`](tools/blueprint/add_custom_event.md) | Add a custom event node with optional typed output parameters |
| [`add_function_call`](tools/blueprint/add_function_call.md) | Add a function call node (by ClassName::FunctionName or auto-discovery) |
| [`add_variable_get`](tools/blueprint/add_variable_get.md) | Add a variable getter node (Blueprint or inherited property) |
| [`add_variable_set`](tools/blueprint/add_variable_set.md) | Add a variable setter (Set) node to a graph |
| [`collapse_to_function`](tools/blueprint/collapse_to_function.md) | Collapse selected nodes into a new function |
| [`collapse_to_macro`](tools/blueprint/collapse_to_macro.md) | Collapse selected nodes into a new macro |
| [`promote_to_variable`](tools/blueprint/promote_to_variable.md) | Promote a pin to a new member variable (getter or setter) |
| [`add_pin`](tools/blueprint/add_pin.md) | Add dynamic pins to Sequence, Switch, MakeArray, DoOnceMultiInput, and math operator nodes |
| [`add_delegate_binding`](tools/blueprint/add_delegate_binding.md) | Create a complete delegate binding (AddDelegate + CreateDelegate + CustomEvent) for any BlueprintAssignable delegate |
| [`add_component_bound_event`](tools/blueprint/add_component_bound_event.md) | Create an event node for a component or widget delegate (ComponentBoundEvent for Actors, FDelegateEditorBinding for Widgets) |

#### Asset Tools (14)

| Tool | Description |
| ---- | ----------- |
| [`list_assets`](tools/asset/list_assets.md) | List/search all asset types via Asset Registry with class, path, name filters and pagination |
| [`get_asset_info`](tools/asset/get_asset_info.md) | Detailed asset metadata: class, size, tags, referencers, dependencies |
| [`rename_asset`](tools/asset/rename_asset.md) | Rename or move any asset type with automatic redirector creation |
| [`move_asset`](tools/asset/move_asset.md) | Move any asset to a different content folder, keeping its name |
| [`duplicate_asset`](tools/asset/duplicate_asset.md) | Duplicate any asset with a new name and optional destination folder |
| [`delete_asset`](tools/asset/delete_asset.md) | Delete any asset with reference checking and optional force mode |
| [`create_folder`](tools/asset/create_folder.md) | Create a content browser folder (with intermediate directories) |
| [`find_references`](tools/asset/find_references.md) | Find all assets referencing a given asset (direct or recursive) |
| [`find_dependencies`](tools/asset/find_dependencies.md) | Find all assets a given asset depends on (direct or recursive) |
| [`fix_redirectors`](tools/asset/fix_redirectors.md) | Fix up and delete ObjectRedirectors in a content path |
| [`import_asset`](tools/asset/import_asset.md) | Import external files (textures, meshes, audio, etc.) into the project |
| [`export_asset`](tools/asset/export_asset.md) | Export an asset to an external file (PNG, FBX, WAV, etc.) |
| [`save_asset`](tools/asset/save_asset.md) | Save a dirty asset's package to disk |
| [`save_all`](tools/asset/save_all.md) | Save all dirty asset packages to disk in one call |

#### Level / World Tools (14)

| Tool | Description |
| ---- | ----------- |
| [`get_level_info`](tools/level/get_level_info.md) | Level name, bounds, streaming levels, actor count by class |
| [`list_actors`](tools/level/list_actors.md) | List actors in the current level with class, tag, folder, and name filters |
| [`get_actor_info`](tools/level/get_actor_info.md) | Detailed actor inspection: properties, components, transform, tags, layers |
| [`spawn_actor`](tools/level/spawn_actor.md) | Spawn an actor from a C++ class or Blueprint asset with transform, label, folder, and tags |
| [`delete_actor`](tools/level/delete_actor.md) | Remove an actor from the level by name or label |
| [`set_actor_transform`](tools/level/set_actor_transform.md) | Move, rotate, and/or scale an actor with partial field updates |
| [`set_actor_property`](tools/level/set_actor_property.md) | Set property values on an actor instance via reflection (dot-notation, arrays) |
| [`get_world_settings`](tools/level/get_world_settings.md) | World settings: game mode, physics/gravity, kill Z, rendering, lightmass, tick |
| [`set_world_settings`](tools/level/set_world_settings.md) | Modify world settings: physics, rendering, lightmass, tick via section.key paths |
| [`get_streaming_levels`](tools/level/get_streaming_levels.md) | List streaming/sub-levels with state, priority, blocking flags, transform, actor count |
| [`load_level`](tools/level/load_level.md) | Open a different level/map in the editor by path or name |
| [`get_selected_actors`](tools/level/get_selected_actors.md) | Get currently selected actors in the editor viewport with optional detail |
| [`select_actors`](tools/level/select_actors.md) | Select actors by name, class, tag, folder, or search filter with visual feedback |
| [`focus_actor`](tools/level/focus_actor.md) | Focus the viewport camera on an actor, framing its bounding box |

#### Material Tools (17)

| Tool | Description |
| ---- | ----------- |
| [`get_material_info`](tools/material/get_material_info.md) | Inspect material properties, parameters, expressions, domain, blend mode, shading model |
| [`list_material_parameters`](tools/material/list_material_parameters.md) | List all parameters of a material/instance by type with values and override status |
| [`create_material`](tools/material/create_material.md) | Create a new material asset with domain, blend mode, shading model, two-sided flag |
| [`create_material_instance`](tools/material/create_material_instance.md) | Create a material instance from a parent with optional scalar, vector, texture parameter overrides |
| [`set_material_parameter`](tools/material/set_material_parameter.md) | Set a scalar, vector, or texture parameter on a material instance |
| [`assign_material`](tools/material/assign_material.md) | Assign a material to a mesh component slot on an actor in the current level |
| [`set_material_properties`](tools/material/set_material_properties.md) | Modify base material properties (domain, blend mode, shading model, two-sided, etc.) |
| [`compile_material`](tools/material/compile_material.md) | Force recompile a material and return errors |
| [`get_material_slots`](tools/material/get_material_slots.md) | Read material slot assignments from an actor's mesh components |
| [`set_static_switch_parameter`](tools/material/set_static_switch_parameter.md) | Set a static switch parameter on a material instance (shader permutation) |
| [`clear_parameter_override`](tools/material/clear_parameter_override.md) | Clear/reset a parameter override on a material instance (revert to parent) |
| [`set_material_instance_parent`](tools/material/set_material_instance_parent.md) | Change the parent material of an existing material instance |
| [`add_material_expression`](tools/material/add_material_expression.md) | Add an expression node to a material graph by class name |
| [`remove_material_expression`](tools/material/remove_material_expression.md) | Remove an expression node from a material graph by GUID or index |
| [`set_material_expression_property`](tools/material/set_material_expression_property.md) | Set properties on a material expression node via reflection |
| [`connect_material_expression`](tools/material/connect_material_expression.md) | Wire expression outputs to material pins or other expression inputs |
| [`disconnect_material_expression`](tools/material/disconnect_material_expression.md) | Break connections at material pins or expression inputs |

#### DataTable / Struct Tools (9)

| Tool | Description |
| ---- | ----------- |
| [`create_data_table`](tools/datatable/create_data_table.md) | Create a new DataTable asset with a specified row struct |
| [`list_data_tables`](tools/datatable/list_data_tables.md) | List DataTable assets with their row struct, row count, and optional filtering |
| [`get_data_table_rows`](tools/datatable/get_data_table_rows.md) | Get all rows (or filtered subset) from a DataTable with field values |
| [`add_data_table_row`](tools/datatable/add_data_table_row.md) | Add a new row to a DataTable with field values |
| [`modify_data_table_row`](tools/datatable/modify_data_table_row.md) | Modify field values of an existing DataTable row |
| [`delete_data_table_row`](tools/datatable/delete_data_table_row.md) | Remove a row from a DataTable by name |
| [`create_user_defined_struct`](tools/datatable/create_user_defined_struct.md) | Create a new UserDefinedStruct asset with initial fields |
| [`modify_user_defined_struct`](tools/datatable/modify_user_defined_struct.md) | Add, remove, rename, or change field types in a UserDefinedStruct |
| [`get_struct_info`](tools/datatable/get_struct_info.md) | Get fields, types, and default values of any UStruct |

#### Widget / UI Tools (26)

| Tool | Description |
| ---- | ----------- |
| [`create_widget_blueprint`](tools/widget/create_widget_blueprint.md) | Create a new UMG Widget Blueprint asset |
| [`list_widget_blueprints`](tools/widget/list_widget_blueprints.md) | List Widget Blueprints with path and name filtering |
| [`list_widget_types`](tools/widget/list_widget_types.md) | List available widget classes for adding to Widget Blueprints |
| [`get_widget_tree`](tools/widget/get_widget_tree.md) | Get the full widget hierarchy of a Widget Blueprint |
| [`get_widget_properties`](tools/widget/get_widget_properties.md) | Get detailed properties of a specific widget |
| [`add_widget`](tools/widget/add_widget.md) | Add a child widget to a panel |
| [`remove_widget`](tools/widget/remove_widget.md) | Remove a widget from the hierarchy |
| [`set_widget_property`](tools/widget/set_widget_property.md) | Modify widget properties (text, color, visibility, etc.) |
| [`set_widget_slot`](tools/widget/set_widget_slot.md) | Modify slot/layout properties (anchors, offsets, padding, etc.) |
| [`reparent_widget`](tools/widget/reparent_widget.md) | Move a widget to a different parent panel |
| [`rename_widget`](tools/widget/rename_widget.md) | Rename a widget (updates event + animation bindings) |
| [`get_widget_animations`](tools/widget/get_widget_animations.md) | List all animations in a Widget Blueprint (name, duration, tracks) |
| [`create_widget_animation`](tools/widget/create_widget_animation.md) | Create a widget animation within a Widget Blueprint |
| [`add_animation_track`](tools/widget/add_animation_track.md) | Add a track to a widget animation (animate opacity, position, color, scale, etc.) |
| [`delete_widget_animation`](tools/widget/delete_widget_animation.md) | Delete an animation from a Widget Blueprint |
| [`get_widget_event_bindings`](tools/widget/get_widget_event_bindings.md) | Get all event bindings for a widget (OnClicked, OnHovered, etc.) |
| [`add_event_binding`](tools/widget/add_event_binding.md) | Bind a widget event to a Blueprint function |
| [`remove_event_binding`](tools/widget/remove_event_binding.md) | Remove an event binding from a widget |
| [`remove_animation_track`](tools/widget/remove_animation_track.md) | Remove a property track from a widget animation |
| [`get_animation_keyframes`](tools/widget/get_animation_keyframes.md) | Get all keyframes of a property track (time, value, interpolation) |
| [`add_animation_keyframe`](tools/widget/add_animation_keyframe.md) | Add or update a keyframe on a property track (value, interpolation) |
| [`remove_animation_keyframe`](tools/widget/remove_animation_keyframe.md) | Remove a keyframe from a property track at a specific time |
| [`set_widget_animation_length`](tools/widget/set_widget_animation_length.md) | Change the duration of a widget animation |
| [`rename_widget_animation`](tools/widget/rename_widget_animation.md) | Rename a widget animation |
| [`fill_named_slot`](tools/widget/fill_named_slot.md) | Move a widget into a named slot of a UserWidget instance |
| [`replace_root_widget`](tools/widget/replace_root_widget.md) | Replace the root widget of a Widget Blueprint, reparenting all children to the new root |

#### Animation Tools (28)

| Tool | Description |
| ---- | ----------- |
| [`list_animation_assets`](tools/animation/list_animation_assets.md) | List AnimSequences, AnimMontages, BlendSpaces, and AnimBPs with filtering by type, skeleton, path, and name |
| [`get_animation_info`](tools/animation/get_animation_info.md) | Detailed inspection of a single animation asset (duration, frames, notifies, curves, skeleton, sections, axes, samples) |
| [`get_anim_blueprint_info`](tools/animation/get_anim_blueprint_info.md) | Deep AnimBP inspection: state machines (states, transitions, entry state), variables, event/function graphs |
| [`get_skeleton_info`](tools/animation/get_skeleton_info.md) | Skeleton bone hierarchy, sockets, virtual bones, animation slots, curves, blend profiles, compatible skeletons |
| [`create_anim_montage`](tools/animation/create_anim_montage.md) | Create an AnimMontage from a skeleton with optional source AnimSequence and slot configuration |
| [`create_blend_space`](tools/animation/create_blend_space.md) | Create a BlendSpace (1D or 2D) with axis settings and target skeleton |
| [`create_anim_blueprint`](tools/animation/create_anim_blueprint.md) | Create an Animation Blueprint for a target skeleton with optional parent class |
| [`add_anim_notify`](tools/animation/add_anim_notify.md) | Add a notify event to an AnimSequence or AnimMontage (custom or typed class, with optional duration) |
| [`remove_anim_notify`](tools/animation/remove_anim_notify.md) | Remove a notify event by name or index from an AnimSequence or AnimMontage |
| [`add_anim_curve`](tools/animation/add_anim_curve.md) | Add a float curve to an AnimSequence (empty; populate with set_anim_curve_keys) |
| [`remove_anim_curve`](tools/animation/remove_anim_curve.md) | Remove a float curve from an AnimSequence |
| [`set_anim_curve_keys`](tools/animation/set_anim_curve_keys.md) | Set keyframes on a float curve with time, value, and interpolation mode |
| [`set_anim_montage_sections`](tools/animation/set_anim_montage_sections.md) | Add, remove, or link montage sections (next-section chaining) |
| [`set_anim_montage_slot`](tools/animation/set_anim_montage_slot.md) | Change the slot name on a montage track |
| [`set_blend_space_axis`](tools/animation/set_blend_space_axis.md) | Configure BlendSpace axis parameters (name, min/max range, grid divisions, snap, wrap) |
| [`add_blend_space_sample`](tools/animation/add_blend_space_sample.md) | Add an AnimSequence sample to a BlendSpace at a position (X, Y) |
| [`remove_blend_space_sample`](tools/animation/remove_blend_space_sample.md) | Remove a sample from a BlendSpace by index or animation reference |
| [`add_anim_state`](tools/animation/add_anim_state.md) | Add a state to a state machine in an Animation Blueprint |
| [`remove_anim_state`](tools/animation/remove_anim_state.md) | Remove a state (and its transitions) from a state machine |
| [`add_anim_transition`](tools/animation/add_anim_transition.md) | Add a transition between two states with crossfade, priority, and bidirectional settings |
| [`remove_anim_transition`](tools/animation/remove_anim_transition.md) | Remove a transition between two states by source and target name |
| [`set_anim_state_animation`](tools/animation/set_anim_state_animation.md) | Set the animation played by a state (creates/updates SequencePlayer node) |
| [`add_anim_bp_variable`](tools/animation/add_anim_bp_variable.md) | Add a variable to an Animation Blueprint (bool, int, float, vector, etc.) |
| [`set_anim_bp_variable`](tools/animation/set_anim_bp_variable.md) | Modify variable properties (default value, category, editability, read-only) |
| [`add_skeleton_socket`](tools/animation/add_skeleton_socket.md) | Add a socket to a skeleton bone (name, transform offset) |
| [`remove_skeleton_socket`](tools/animation/remove_skeleton_socket.md) | Remove a socket from a skeleton by name |
| [`add_virtual_bone`](tools/animation/add_virtual_bone.md) | Add a virtual bone between a source and target bone |
| [`remove_virtual_bone`](tools/animation/remove_virtual_bone.md) | Remove a virtual bone by name |

#### PCG Query Tools (4)

| Tool | Description |
| ---- | ----------- |
| [`list_pcg_graphs`](tools/pcg/list_pcg_graphs.md) | List PCG graph assets with path, name, node count, and generation settings |
| [`get_pcg_graph_info`](tools/pcg/get_pcg_graph_info.md) | Detailed PCG graph inspection: nodes, pins, edges, hierarchical generation |
| [`get_pcg_node`](tools/pcg/get_pcg_node.md) | Deep single-node inspection: settings, seed, pins with connections, overridable params |
| [`get_pcg_component_info`](tools/pcg/get_pcg_component_info.md) | Inspect PCG component on an actor: graph reference, generation status, grid size |

#### PCG Lifecycle Tools (2)

| Tool | Description |
| ---- | ----------- |
| [`create_pcg_graph`](tools/pcg/create_pcg_graph.md) | Create a new PCG Graph asset (saved to disk, registered with Asset Registry) |
| [`add_pcg_component`](tools/pcg/add_pcg_component.md) | Add/update a PCG Component on an actor and optionally assign a graph |

#### PCG Graph Editing Tools (5)

| Tool | Description |
| ---- | ----------- |
| [`add_pcg_node`](tools/pcg/add_pcg_node.md) | Add a node to a PCG Graph by settings class name (e.g. SurfaceSampler, StaticMeshSpawner) |
| [`remove_pcg_node`](tools/pcg/remove_pcg_node.md) | Remove a node from a PCG Graph |
| [`connect_pcg_nodes`](tools/pcg/connect_pcg_nodes.md) | Wire two PCG nodes together (output pin to input pin) |
| [`disconnect_pcg_nodes`](tools/pcg/disconnect_pcg_nodes.md) | Break a wire between two PCG nodes |
| [`set_pcg_node_settings`](tools/pcg/set_pcg_node_settings.md) | Set settings/properties on a PCG node via reflection |

#### PCG Execution Tools (2)

| Tool | Description |
| ---- | ----------- |
| [`execute_pcg_graph`](tools/pcg/execute_pcg_graph.md) | Trigger PCG graph generation on an actor's PCG component (async) |
| [`clear_pcg_generation`](tools/pcg/clear_pcg_generation.md) | Clear/cleanup all generated content from a PCG component |

#### Niagara Query Tools (3)

| Tool | Description |
| ---- | ----------- |
| [`list_niagara_assets`](tools/niagara/list_niagara_assets.md) | List Niagara System and Emitter assets (filter by path, name, type) |
| [`get_niagara_system_info`](tools/niagara/get_niagara_system_info.md) | Inspect a Niagara System: emitters, exposed parameters, bounds, warmup |
| [`get_niagara_emitter_info`](tools/niagara/get_niagara_emitter_info.md) | Inspect a Niagara Emitter: renderers, sim target, scripts, simulation stages |

#### Niagara Lifecycle Tools (2)

| Tool | Description |
| ---- | ----------- |
| [`create_niagara_system`](tools/niagara/create_niagara_system.md) | Create a new Niagara System asset (empty or from a template system) |
| [`create_niagara_emitter`](tools/niagara/create_niagara_emitter.md) | Create a new Niagara Emitter asset (empty or from a template emitter) |

#### Niagara Editing Tools (6)

| Tool | Description |
| ---- | ----------- |
| [`add_niagara_emitter_to_system`](tools/niagara/add_niagara_emitter_to_system.md) | Add an emitter reference to a Niagara System |
| [`remove_niagara_emitter_from_system`](tools/niagara/remove_niagara_emitter_from_system.md) | Remove an emitter from a Niagara System by name |
| [`get_niagara_parameters`](tools/niagara/get_niagara_parameters.md) | List all user-exposed parameters with types and current values |
| [`set_niagara_parameter`](tools/niagara/set_niagara_parameter.md) | Set a user-exposed parameter (float, int, bool, vector, color) |
| [`set_niagara_emitter_property`](tools/niagara/set_niagara_emitter_property.md) | Set emitter properties (sim target, bounds, allocation, etc.) |
| [`set_niagara_renderer`](tools/niagara/set_niagara_renderer.md) | Configure renderer settings (enabled, material, UPROPERTY values) |

#### Sequencer Tools (13)

| Tool | Description | Key Parameters |
| ---- | ----------- | -------------- |
| [`list_level_sequences`](tools/sequencer/list_level_sequences.md) | List Level Sequence assets | `path_prefix`, `name_filter`, `include_engine` |
| [`get_level_sequence_info`](tools/sequencer/get_level_sequence_info.md) | Inspect a Level Sequence | `asset_path`, `include_bindings`, `include_tracks` |
| [`get_sequence_track`](tools/sequencer/get_sequence_track.md) | Detailed track inspection | `asset_path`, `binding_name`, `track_index`, `include_keyframes`, `max_keyframes` |
| [`create_level_sequence`](tools/sequencer/create_level_sequence.md) | Create a new Level Sequence asset | `name`, `path`, `frame_rate`, `duration` |
| [`add_sequence_binding`](tools/sequencer/add_sequence_binding.md) | Bind an actor to a Level Sequence | `asset_path`, `actor_name`, `binding_type` |
| [`remove_sequence_binding`](tools/sequencer/remove_sequence_binding.md) | Remove a binding from a Level Sequence | `asset_path`, `binding_name` |
| [`add_sequence_track`](tools/sequencer/add_sequence_track.md) | Add a track to a binding or master | `asset_path`, `binding_name`, `track_type` |
| [`remove_sequence_track`](tools/sequencer/remove_sequence_track.md) | Remove a track from a sequence | `asset_path`, `binding_name`, `track_index` |
| [`add_sequence_section`](tools/sequencer/add_sequence_section.md) | Add a section to a track | `asset_path`, `track_index`, `binding_name`, `start_time`, `end_time` |
| [`remove_sequence_section`](tools/sequencer/remove_sequence_section.md) | Remove a section from a track | `asset_path`, `track_index`, `section_index`, `binding_name` |
| [`add_sequence_keyframe`](tools/sequencer/add_sequence_keyframe.md) | Add/set a keyframe on a channel | `asset_path`, `track_index`, `time`, `value`, `channel_index`, `interpolation` |
| [`remove_sequence_keyframe`](tools/sequencer/remove_sequence_keyframe.md) | Remove a keyframe from a channel | `asset_path`, `track_index`, `time`, `channel_index` |
| [`set_sequence_playback`](tools/sequencer/set_sequence_playback.md) | Set playback range, frame rate, view/working range | `asset_path`, `start_time`, `end_time`, `frame_rate` |

#### Behavior Tree Tools (7)

| Tool | Description |
| ---- | ----------- |
| [`list_behavior_trees`](tools/behaviortree/list_behavior_trees.md) | List Behavior Tree assets with path and name filtering |
| [`get_behavior_tree_info`](tools/behaviortree/get_behavior_tree_info.md) | Inspect a BT: root node hierarchy, tasks, decorators, services, blackboard |
| [`create_behavior_tree`](tools/behaviortree/create_behavior_tree.md) | Create a new BT asset with root composite (Selector, Sequence, SimpleParallel) |
| [`add_bt_node`](tools/behaviortree/add_bt_node.md) | Add a node to a BT (Composite, Task, Decorator, Service) by path |
| [`remove_bt_node`](tools/behaviortree/remove_bt_node.md) | Remove a node from a BT by path |
| [`set_bt_node_property`](tools/behaviortree/set_bt_node_property.md) | Set a property on a BT node via reflection |
| [`connect_bt_nodes`](tools/behaviortree/connect_bt_nodes.md) | Move/re-parent a BT node to a new parent composite |

#### Blackboard Tools (6)

| Tool | Description |
| ---- | ----------- |
| [`list_blackboards`](tools/blackboard/list_blackboards.md) | List Blackboard Data assets with path and name filtering |
| [`get_blackboard_info`](tools/blackboard/get_blackboard_info.md) | Inspect a Blackboard: keys (name, type, instance synced, description), parent chain |
| [`create_blackboard`](tools/blackboard/create_blackboard.md) | Create a new Blackboard Data asset with optional parent for key inheritance |
| [`add_blackboard_key`](tools/blackboard/add_blackboard_key.md) | Add a key (Bool, Int, Float, String, Name, Vector, Rotator, Enum, Object, Class) |
| [`remove_blackboard_key`](tools/blackboard/remove_blackboard_key.md) | Remove a key from a Blackboard by name |
| [`set_blackboard_key_properties`](tools/blackboard/set_blackboard_key_properties.md) | Modify key properties (instance synced, description, category) |

#### State Tree Tools (7)

| Tool | Description |
| ---- | ----------- |
| [`list_state_trees`](tools/statetree/list_state_trees.md) | List State Tree assets with path and name filtering |
| [`get_state_tree_info`](tools/statetree/get_state_tree_info.md) | Inspect a State Tree: states, transitions, tasks, conditions, evaluators |
| [`create_state_tree`](tools/statetree/create_state_tree.md) | Create a new State Tree asset with configurable schema |
| [`add_state_tree_state`](tools/statetree/add_state_tree_state.md) | Add a state (State, Group, Linked, Subtree) as root or child |
| [`remove_state_tree_state`](tools/statetree/remove_state_tree_state.md) | Remove a state and its children from a State Tree |
| [`add_state_tree_transition`](tools/statetree/add_state_tree_transition.md) | Add a transition between states (trigger, priority, delay) |
| [`set_state_tree_task`](tools/statetree/set_state_tree_task.md) | Set/configure a task on a state (class, properties, enabled) |

#### EQS Tools (5)

| Tool | Description |
| ---- | ----------- |
| [`list_eqs_queries`](tools/eqs/list_eqs_queries.md) | List EQS Query template assets with path and name filtering |
| [`get_eqs_query_info`](tools/eqs/get_eqs_query_info.md) | Inspect an EQS Query: generators, tests, options, weighting |
| [`create_eqs_query`](tools/eqs/create_eqs_query.md) | Create a new EQS Query template asset |
| [`add_eqs_generator`](tools/eqs/add_eqs_generator.md) | Add a generator to an EQS Query (Points Around, Actors Of Class, Grid, etc.) |
| [`add_eqs_test`](tools/eqs/add_eqs_test.md) | Add a test to an EQS Query (Distance, Trace, Dot, PathFinding, etc.) |

#### Smart Object Tools (5)

| Tool | Description |
| ---- | ----------- |
| [`list_smart_object_definitions`](tools/smartobject/list_smart_object_definitions.md) | List Smart Object Definition assets with path and name filtering |
| [`get_smart_object_definition_info`](tools/smartobject/get_smart_object_definition_info.md) | Inspect a Smart Object Definition: slots, offsets, tags, policies |
| [`create_smart_object_definition`](tools/smartobject/create_smart_object_definition.md) | Create a new Smart Object Definition asset |
| [`add_smart_object_slot`](tools/smartobject/add_smart_object_slot.md) | Add a slot to a Smart Object Definition (offset, rotation, tags, enabled) |
| [`remove_smart_object_slot`](tools/smartobject/remove_smart_object_slot.md) | Remove a slot from a Smart Object Definition by index |

#### Static Mesh Tools (6)

| Tool | Description |
| ---- | ----------- |
| [`get_static_mesh_info`](tools/mesh/get_static_mesh_info.md) | Inspect a Static Mesh: LODs, materials, sockets, bounds, Nanite, collision, lightmap |
| [`set_static_mesh_property`](tools/mesh/set_static_mesh_property.md) | Set Static Mesh properties (lightmap_resolution, nanite_enabled, lod_group, min_lod) |
| [`add_static_mesh_socket`](tools/mesh/add_static_mesh_socket.md) | Add a socket to a Static Mesh |
| [`remove_static_mesh_socket`](tools/mesh/remove_static_mesh_socket.md) | Remove a socket from a Static Mesh by name |
| [`set_static_mesh_collision`](tools/mesh/set_static_mesh_collision.md) | Configure collision settings (complexity, LOD for collision) |
| [`set_static_mesh_material`](tools/mesh/set_static_mesh_material.md) | Assign a material to a Static Mesh material slot |

#### Skeletal Mesh Tools (3)

| Tool | Description |
| ---- | ----------- |
| [`get_skeletal_mesh_info`](tools/mesh/get_skeletal_mesh_info.md) | Inspect a Skeletal Mesh: LODs, materials, skeleton, physics asset, morph targets, bounds, sockets |
| [`set_skeletal_mesh_property`](tools/mesh/set_skeletal_mesh_property.md) | Set Skeletal Mesh properties (physics_asset, min_lod) |
| [`set_skeletal_mesh_material`](tools/mesh/set_skeletal_mesh_material.md) | Assign a material to a Skeletal Mesh material slot |

#### Mesh Operations (3)

| Tool | Description |
| ---- | ----------- |
| [`merge_meshes`](tools/mesh/merge_meshes.md) | Merge multiple Static Mesh actors into a single Static Mesh asset |
| [`generate_lods`](tools/mesh/generate_lods.md) | Auto-generate LODs for a Static Mesh or Skeletal Mesh |
| [`generate_collision`](tools/mesh/generate_collision.md) | Auto-generate collision for a Static Mesh (box, sphere, capsule, convex) |

#### Enhanced Input Tools (8)

| Tool | Description |
| ---- | ----------- |
| [`list_input_mapping_contexts`](tools/enhanced-input/list_input_mapping_contexts.md) | List Input Mapping Context assets with mapping counts |
| [`get_input_mapping_context_info`](tools/enhanced-input/get_input_mapping_context_info.md) | Inspect an IMC: action mappings, keys, triggers, modifiers |
| [`create_input_action`](tools/enhanced-input/create_input_action.md) | Create a new Input Action asset (Boolean, Axis1D, Axis2D, Axis3D) |
| [`create_input_mapping_context`](tools/enhanced-input/create_input_mapping_context.md) | Create a new Input Mapping Context asset |
| [`add_input_mapping`](tools/enhanced-input/add_input_mapping.md) | Add an action mapping to a context (action, key, triggers, modifiers) |
| [`remove_input_mapping`](tools/enhanced-input/remove_input_mapping.md) | Remove an action mapping from a context by index |
| [`set_input_action_property`](tools/enhanced-input/set_input_action_property.md) | Set Input Action properties (value type, description, consume input, triggers, modifiers) |
| [`list_input_actions`](tools/enhanced-input/list_input_actions.md) | List Enhanced Input Actions and Input Mapping Contexts with action names, value types, triggers, modifiers |

#### GAS - Abilities & Effects (13)

| Tool | Description |
| ---- | ----------- |
| [`list_gameplay_abilities`](tools/gas/list_gameplay_abilities.md) | List Gameplay Ability Blueprint assets: tags, instancing policy, net execution policy |
| [`get_gameplay_ability_info`](tools/gas/get_gameplay_ability_info.md) | Inspect a Gameplay Ability: policies, all tag containers, cost/cooldown effects |
| [`list_gameplay_effects`](tools/gas/list_gameplay_effects.md) | List Gameplay Effect Blueprint assets: duration, modifiers, stacking, tags |
| [`get_gameplay_effect_info`](tools/gas/get_gameplay_effect_info.md) | Inspect a Gameplay Effect: modifiers, duration, stacking, tags, cues, components |
| [`create_gameplay_ability`](tools/gas/create_gameplay_ability.md) | Create a new Gameplay Ability Blueprint (custom parent class supported) |
| [`create_gameplay_effect`](tools/gas/create_gameplay_effect.md) | Create a new Gameplay Effect Blueprint (Instant/Infinite/HasDuration) |
| [`set_gameplay_effect_modifier`](tools/gas/set_gameplay_effect_modifier.md) | Add or edit a modifier on a Gameplay Effect (attribute, op, magnitude) |
| [`list_attribute_sets`](tools/gas/list_attribute_sets.md) | List Attribute Set classes (C++ and Blueprint): name, origin, attribute count |
| [`get_attribute_set_info`](tools/gas/get_attribute_set_info.md) | Inspect an Attribute Set: attributes with name, base value, current value |
| [`create_attribute_set`](tools/gas/create_attribute_set.md) | Create a new Attribute Set Blueprint with initial FGameplayAttributeData attributes |
| [`add_gameplay_tag`](tools/gas/add_gameplay_tag.md) | Add a new Gameplay Tag to the project INI (idempotent if tag exists) |
| [`remove_gameplay_tag`](tools/gas/remove_gameplay_tag.md) | Remove a Gameplay Tag from the project INI |
| [`rename_gameplay_tag`](tools/gas/rename_gameplay_tag.md) | Rename a Gameplay Tag in the project INI (creates a redirector) |

#### Audio / MetaSounds (19)

| Tool | Description |
| ---- | ----------- |
| [`list_audio_assets`](tools/audio/list_audio_assets.md) | List Sound Waves, Sound Cues, MetaSounds, Sound Classes, Sound Mixes, Attenuation Settings |
| [`get_sound_cue_info`](tools/audio/get_sound_cue_info.md) | Inspect a Sound Cue: nodes, connections, volume/pitch, attenuation, sound class, duration |
| [`get_metasound_info`](tools/audio/get_metasound_info.md) | Inspect a MetaSound Source: metadata, inputs, outputs, graph pages, nodes, edges, dependencies |
| [`create_sound_cue`](tools/audio/create_sound_cue.md) | Create a new Sound Cue asset (optional volume/pitch multipliers) |
| [`create_metasound`](tools/audio/create_metasound.md) | Create a new MetaSound Source asset with default graph |
| [`create_sound_class`](tools/audio/create_sound_class.md) | Create a new Sound Class asset (optional volume/pitch) |
| [`create_sound_mix`](tools/audio/create_sound_mix.md) | Create a new Sound Mix asset |
| [`create_sound_attenuation`](tools/audio/create_sound_attenuation.md) | Create a new Sound Attenuation Settings asset (optional shape, radius, falloff) |
| [`add_sound_cue_node`](tools/audio/add_sound_cue_node.md) | Add a node to a Sound Cue graph (WavePlayer, Mixer, Random, Modulator, Delay, etc.) |
| [`remove_sound_cue_node`](tools/audio/remove_sound_cue_node.md) | Remove a node from a Sound Cue graph by index |
| [`connect_sound_cue_nodes`](tools/audio/connect_sound_cue_nodes.md) | Wire two Sound Cue nodes together (parent -> child) |
| [`set_sound_cue_node_property`](tools/audio/set_sound_cue_node_property.md) | Set type-specific properties on a Sound Cue node |
| [`add_metasound_node`](tools/audio/add_metasound_node.md) | Add a node to a MetaSound graph by class name (e.g. UE.Add.Float) |
| [`remove_metasound_node`](tools/audio/remove_metasound_node.md) | Remove a node from a MetaSound graph by GUID |
| [`connect_metasound_nodes`](tools/audio/connect_metasound_nodes.md) | Wire two MetaSound nodes together (output -> input) |
| [`set_metasound_input`](tools/audio/set_metasound_input.md) | Set the default value of a MetaSound node input |
| [`set_sound_class_properties`](tools/audio/set_sound_class_properties.md) | Set Sound Class properties (volume, pitch, LPF, attenuation scale, flags) |
| [`set_sound_mix_properties`](tools/audio/set_sound_mix_properties.md) | Set Sound Mix properties (EQ, timing, sound class effect overrides) |
| [`set_sound_attenuation`](tools/audio/set_sound_attenuation.md) | Set Sound Attenuation settings (shape, distance, spatialization, occlusion) |

#### Landscape / Terrain (9)

| Tool | Description |
| ---- | ----------- |
| [`get_landscape_info`](tools/landscape/get_landscape_info.md) | Inspect a Landscape actor: size, components, layers, material, LOD, collision, Nanite, edit layers |
| [`list_landscape_layers`](tools/landscape/list_landscape_layers.md) | List Landscape Layer Info assets: layer name, hardness, physical material, weight-blend mode |
| [`create_landscape`](tools/landscape/create_landscape.md) | Create a new flat Landscape actor with configurable grid size, section layout, position, and scale |
| [`create_landscape_layer_info`](tools/landscape/create_landscape_layer_info.md) | Create a new Landscape Layer Info asset for weight-based painting layers |
| [`set_landscape_material`](tools/landscape/set_landscape_material.md) | Assign a material (and optional hole material) to a Landscape actor |
| [`set_landscape_property`](tools/landscape/set_landscape_property.md) | Set Landscape properties (LOD, collision, navigation, Nanite, streaming) |
| [`export_landscape_heightmap`](tools/landscape/export_landscape_heightmap.md) | Export the Landscape heightmap to a raw uint16 binary file |
| [`import_landscape_heightmap`](tools/landscape/import_landscape_heightmap.md) | Import a raw uint16 heightmap file onto an existing Landscape |
| [`import_landscape_layer_weight`](tools/landscape/import_landscape_layer_weight.md) | Import a raw uint8 weight map for a paint layer |

#### Physics Material Tools (4)

| Tool | Description |
| ---- | ----------- |
| [`list_physics_materials`](tools/physics/list_physics_materials.md) | List Physical Material assets: friction, restitution, density, surface type |
| [`get_physics_material_info`](tools/physics/get_physics_material_info.md) | Inspect a Physical Material: all properties, combine modes, sleep thresholds, strength |
| [`create_physics_material`](tools/physics/create_physics_material.md) | Create a new Physical Material asset with optional initial properties |
| [`set_physics_material_property`](tools/physics/set_physics_material_property.md) | Set Physical Material properties (friction, restitution, density, surface type, strength, etc.) |

#### Physics Asset Tools (8)

| Tool | Description |
| ---- | ----------- |
| [`get_physics_asset_info`](tools/physics/get_physics_asset_info.md) | Inspect a Physics Asset: bodies (bone, shape, mass, physics type), constraints (limits, motion types) |
| [`create_physics_asset`](tools/physics/create_physics_asset.md) | Create a new empty Physics Asset |
| [`add_physics_body`](tools/physics/add_physics_body.md) | Add a physics body with collision shape (sphere, box, capsule) to a Physics Asset |
| [`remove_physics_body`](tools/physics/remove_physics_body.md) | Remove a physics body by bone name or index (cascades constraint removal) |
| [`set_physics_body_property`](tools/physics/set_physics_body_property.md) | Set body properties (mass, damping, physics type, simulate, gravity, collision profile) |
| [`add_physics_constraint`](tools/physics/add_physics_constraint.md) | Add a constraint between two bodies with swing/twist limits |
| [`remove_physics_constraint`](tools/physics/remove_physics_constraint.md) | Remove a constraint from a Physics Asset by index |
| [`set_physics_constraint_property`](tools/physics/set_physics_constraint_property.md) | Set constraint properties (motion types, limits, stiffness, damping, collision) |

#### Foliage Tools (7)

| Tool | Description |
| ---- | ----------- |
| [`list_foliage_types`](tools/foliage/list_foliage_types.md) | List Foliage Type assets with optional path/name filtering and summary info |
| [`get_foliage_type_info`](tools/foliage/get_foliage_type_info.md) | Detailed Foliage Type inspection: mesh, painting, placement, instance settings, scalability |
| [`get_foliage_instances`](tools/foliage/get_foliage_instances.md) | Query foliage instances in the current level with optional type and bounding box filters |
| [`create_foliage_type`](tools/foliage/create_foliage_type.md) | Create a new Foliage Type (Instanced Static Mesh) from a Static Mesh with optional properties |
| [`set_foliage_type_property`](tools/foliage/set_foliage_type_property.md) | Set Foliage Type properties: density, scaling, placement, shadows, culling, mobility |
| [`add_foliage_instances`](tools/foliage/add_foliage_instances.md) | Programmatically place foliage instances with location, rotation, and scale |
| [`remove_foliage_instances`](tools/foliage/remove_foliage_instances.md) | Remove foliage instances by type, bounding box area, or all |

#### World Partition Tools (7)

| Tool | Description |
| ---- | ----------- |
| [`get_world_partition_info`](tools/worldpartition/get_world_partition_info.md) | Inspect World Partition settings: enabled state, streaming, grid info, HLOD defaults, data layer summary |
| [`list_data_layers`](tools/worldpartition/list_data_layers.md) | List all Data Layers with name, type, initial runtime state, visibility, and hierarchy |
| [`get_hlod_info`](tools/worldpartition/get_hlod_info.md) | Get HLOD settings: default layer, all HLOD layer assets with configuration |
| [`create_data_layer`](tools/worldpartition/create_data_layer.md) | Create a new Data Layer asset and register it with the world's data layer system |
| [`set_data_layer_state`](tools/worldpartition/set_data_layer_state.md) | Set Data Layer state: editor visibility, loaded-in-editor, initial runtime state |
| [`set_actor_data_layer`](tools/worldpartition/set_actor_data_layer.md) | Add or remove an actor from a Data Layer |
| [`set_world_partition_settings`](tools/worldpartition/set_world_partition_settings.md) | Configure World Partition settings: streaming, default HLOD layer |

#### Control Rig Tools (6)

| Tool | Description |
| ---- | ----------- |
| [`list_control_rigs`](tools/controlrig/list_control_rigs.md) | List Control Rig Blueprint assets with optional name/path filtering |
| [`get_control_rig_info`](tools/controlrig/get_control_rig_info.md) | Inspect hierarchy elements (bones, controls, nulls, curves), transforms, and control settings |
| [`create_control_rig`](tools/controlrig/create_control_rig.md) | Create a new Control Rig Blueprint, optionally from a Skeleton |
| [`add_control_rig_element`](tools/controlrig/add_control_rig_element.md) | Add an element (Bone, Control, Null, Curve) to a Control Rig's hierarchy |
| [`remove_control_rig_element`](tools/controlrig/remove_control_rig_element.md) | Remove an element from a Control Rig's hierarchy |
| [`set_control_rig_element_property`](tools/controlrig/set_control_rig_element_property.md) | Set properties on a hierarchy element: transform, parent, shape, color |

#### Rendering Configuration Tools (6)

| Tool | Description |
| ---- | ----------- |
| [`get_rendering_settings`](tools/rendering/get_rendering_settings.md) | Get global rendering settings (GI, Reflections, Shadows, AA, Nanite, Ray Tracing) |
| [`set_rendering_settings`](tools/rendering/set_rendering_settings.md) | Modify global rendering settings via console variables |
| [`get_post_process_settings`](tools/rendering/get_post_process_settings.md) | Get Post Process Volume settings (bloom, exposure, DOF, etc.) |
| [`set_post_process_settings`](tools/rendering/set_post_process_settings.md) | Set Post Process Volume settings with auto-override flags |
| [`set_nanite_settings`](tools/rendering/set_nanite_settings.md) | Configure Nanite per-mesh or global settings |
| [`set_lumen_settings`](tools/rendering/set_lumen_settings.md) | Configure Lumen GI and reflection settings via CVars |

#### Curve Asset Tools (6)

| Tool | Description |
| ---- | ----------- |
| [`list_curve_assets`](tools/curve/list_curve_assets.md) | List Curve Float, Curve Vector, Curve Linear Color, and Curve Table assets |
| [`get_curve_asset_info`](tools/curve/get_curve_asset_info.md) | Inspect a Curve asset: keys, time/value ranges, type, optional evaluation |
| [`create_curve_asset`](tools/curve/create_curve_asset.md) | Create a new Curve asset (Float, Vector, or LinearColor) with optional initial keys |
| [`set_curve_keys`](tools/curve/set_curve_keys.md) | Set keyframes on a Curve asset (replace all or add/update individual keys) |
| [`create_curve_table`](tools/curve/create_curve_table.md) | Create a new empty Curve Table asset |
| [`add_curve_table_row`](tools/curve/add_curve_table_row.md) | Add a named row (curve) to a Curve Table with optional initial keys |

#### Motion Design Tools (5)

| Tool | Description |
| ---- | ----------- |
| [`get_motion_design_info`](tools/motiondesign/get_motion_design_info.md) | Inspect Cloners and Effectors, or list all Motion Design actors in the level |
| [`create_motion_design_actor`](tools/motiondesign/create_motion_design_actor.md) | Spawn a Cloner or Effector actor with optional layout, seed, and transform |
| [`set_motion_design_property`](tools/motiondesign/set_motion_design_property.md) | Set properties on a Cloner or Effector (enabled, seed, layout, magnitude, etc.) |
| [`add_motion_design_effector`](tools/motiondesign/add_motion_design_effector.md) | Link an Effector to a Cloner so it affects the cloner's instances |
| [`add_motion_design_modifier`](tools/motiondesign/add_motion_design_modifier.md) | Set effector shape/type and mode on an Effector actor |

#### Project / Editor Info Tools (18)

| Tool | Description |
| ---- | ----------- |
| [`get_project_info`](tools/project/get_project_info.md) | Project name, engine version, target platforms, enabled plugins, maps, project modules |
| [`list_plugins`](tools/project/list_plugins.md) | List all discovered plugins with version, description, category, type, and enabled status |
| [`list_modules`](tools/project/list_modules.md) | List all modules with loaded status, game module flag, and file path |
| [`get_editor_state`](tools/project/get_editor_state.md) | PIE status, loaded level, selected actors, editor mode, viewport camera, dirty packages |
| [`get_editor_preferences`](tools/project/get_editor_preferences.md) | Read editor INI config settings by section and key |
| [`set_editor_preferences`](tools/project/set_editor_preferences.md) | Write editor INI config settings by section and key |
| [`get_class_hierarchy`](tools/project/get_class_hierarchy.md) | Inheritance chain for any UClass (from given class up to UObject) |
| [`get_class_info`](tools/project/get_class_info.md) | Properties and functions of a native or Blueprint class (name, type, flags, category) |
| [`get_enum_values`](tools/project/get_enum_values.md) | Values and display names of a UEnum |
| [`list_asset_types`](tools/project/list_asset_types.md) | List all registered UClass types that can exist as assets |
| [`get_project_settings`](tools/project/get_project_settings.md) | Read project configuration from Default*.ini files (DefaultEngine, DefaultGame, etc.) |
| [`set_project_settings`](tools/project/set_project_settings.md) | Write/modify project configuration in Default*.ini files |
| [`get_collision_profiles`](tools/project/get_collision_profiles.md) | List collision presets and channels with object types and responses |
| [`list_gameplay_tags`](tools/project/list_gameplay_tags.md) | List all gameplay tags defined in the project with hierarchy info |
| [`execute_console_command`](tools/project/execute_console_command.md) | Run an Unreal console command and capture the output |
| [`get_log_output`](tools/project/get_log_output.md) | Retrieve recent Output Log entries with category and verbosity filtering |
| [`live_compile`](tools/project/live_compile.md) | Trigger Live Coding (hot reload) to recompile C++ while the editor is running |
| [`list_tools`](tools/project/list_tools.md) | List all registered MCP tools with name, description, and category |

#### Infrastructure Tools (1)

| Tool | Description |
| ---- | ----------- |
| [`get_request_log`](tools/infrastructure/get_request_log.md) | Query history of all MCP tool calls with filtering, limits, and optional full detail |

#### PIE / Testing Tools (10)

| Tool | Description |
| ---- | ----------- |
| [`start_pie`](tools/pie/start_pie.md) | Start a Play In Editor session (viewport, new window, or simulate mode) |
| [`stop_pie`](tools/pie/stop_pie.md) | Stop the current PIE session |
| [`is_pie_running`](tools/pie/is_pie_running.md) | Check PIE session state, paused status, elapsed time, player info |
| [`pause_pie`](tools/pie/pause_pie.md) | Pause or unpause the current PIE session |
| [`take_screenshot`](tools/pie/take_screenshot.md) | Capture a screenshot of the editor or PIE viewport |
| [`inject_input`](tools/pie/inject_input.md) | Simulate keyboard/mouse input or execute console commands in PIE |
| [`get_fps`](tools/pie/get_fps.md) | Get frame rate and frame time statistics |
| [`get_viewport_info`](tools/pie/get_viewport_info.md) | Get editor viewport camera position, rotation, FOV, resolution |
| [`set_viewport_camera`](tools/pie/set_viewport_camera.md) | Move the editor viewport camera position, rotation, and FOV |
| [`run_automation_test`](tools/pie/run_automation_test.md) | List available automation tests or run a specific test by name |

---

## Architecture

```
+---------------------------------------------------+
|                 Unreal Editor                      |
|                                                    |
|  +---------------+   +-------------------------+  |
|  | FMCPHttpServer |-->| FMCPProtocolHandler     |  |
|  | FMCPTlsServer  |   |  - JSON-RPC 2.0         |  |
|  |  (port 8090)  |   |  - Session management   |  |
|  |  - POST /mcp  |   |  - Tool registry        |  |
|  |  - GET health |   |  - Tool dispatch         |  |
|  +---------------+   +-----+---+---+-----------+  |
|                             |   |   |              |
|       +---------------------+   |   +------+       |
|       v                         v          v       |
|  Core Tools (273)       Extension Modules (87)     |
|  (always loaded)        (loaded at startup if      |
|  Blueprint, Asset,       dependency is present)    |
|  Level, Material, …     PCG, Niagara, GAS, …      |
|                                                     |
|  +---------------+  +---------------------------+  |
|  |FMCPRequestLog |  | UMCPCoreSettings          |  |
|  |  Thread-safe  |  |  Port, AutoStart, CORS,   |  |
|  |  logging      |  |  API Key, Rate Limiting   |  |
|  +---------------+  +---------------------------+  |
+-----------------------------------------------------+
```

## Optional Plugin Dependencies

Domain-specific tool modules (PCG, Niagara, StateTree, SmartObjects, GAS, ControlRig, MetaSounds, EnhancedInput, MotionDesign) are isolated into **separate extension DLLs**, one per optional dependency. The main plugin DLL (`UnrealMCPCore.dll`) has **zero optional plugin dependencies** and always loads successfully.

At startup, the main module attempts to load each extension module via `FModuleManager::LoadModule()`. If an extension's dependency plugin is not installed (e.g. a Fab user without GameplayAbilities), that extension DLL simply fails to load and the main module continues. All other tools remain available. This is critical for Fab distribution where users may not have all plugins installed.

| Extension Module | Plugin Required | Tools Provided |
| --- | --- | --- |
| `UnrealMCPCorePCG` | PCG | 13 PCG tools |
| `UnrealMCPCoreNiagara` | Niagara | 11 Niagara tools |
| `UnrealMCPCoreStateTree` | StateTree | 7 State Tree tools |
| `UnrealMCPCoreSmartObject` | SmartObjects | 5 Smart Object tools |
| `UnrealMCPCoreEnhancedInput` | EnhancedInput | 8 Enhanced Input tools |
| `UnrealMCPCoreGAS` | GameplayAbilities | 13 GAS tools |
| `UnrealMCPCoreMetaSound` | Metasound | 19 Audio/MetaSound tools |
| `UnrealMCPCoreControlRig` | ControlRig | 6 Control Rig tools |
| `UnrealMCPCoreMotionDesign` | ClonerEffector | 5 Motion Design tools |

> **Note on Beta / Experimental Dependencies:**
> Some extension modules depend on engine plugins that Epic Games marks as pre-release:
>
> | Extension Module | Required Plugin | Stability in UE 5.5 |
> | --- | --- | --- |
> | `UnrealMCPCorePCG` | PCG | **Beta** |
> | `UnrealMCPCoreMotionDesign` | ClonerEffector | **Experimental** |
>
> These designations are set by Epic and may change in future engine versions. Because each extension is an isolated DLL, a breaking change in a Beta/Experimental plugin only affects that one module. All other tools remain available. If you encounter issues with these modules after an engine update, disable them in the MCP Core plugin settings until a compatible update is released.

See the project README for the full technical details on the extension module architecture.

For the full feature roadmap and checklist, see the project's **README_TODO.md**.

## Custom Tools

UnrealMCPCore is designed to be extended. You can add your own project-specific MCP tools using the same declarative pattern the built-in tools use. No schema building or manual registration required.

### How It Works

1. Create a C++ class that inherits from `UMCPToolset`
2. Add `static` `UFUNCTION`s. Each one becomes an MCP tool automatically
3. The plugin discovers your toolset via reflection at startup

### Minimal Example

**Header** (`MyProjectToolset.h`):

```cpp
#pragma once
#include "Toolset/MCPToolset.h"
#include "MyProjectToolset.generated.h"

UCLASS(Hidden)
class UMyProjectToolset : public UMCPToolset
{
    GENERATED_BODY()
public:
    /**
     * Returns the current health value for the given character Blueprint.
     * @param blueprint_path  Asset path to the character Blueprint
     */
    UFUNCTION(BlueprintCallable, Category = "MyProject")
    static FMCPToolsetResult get_character_health(const FString& blueprint_path);

    /**
     * Sets the max health on a character Blueprint.
     * @param blueprint_path  Asset path to the character Blueprint
     * @param max_health      New max health value
     */
    UFUNCTION(BlueprintCallable, Category = "MyProject",
        meta = (Mutating))
    static FMCPToolsetResult set_character_max_health(
        const FString& blueprint_path, float max_health);
};
```

**Implementation** (`MyProjectToolset.cpp`):

```cpp
#include "MyProjectToolset.h"
#include "Core/MCPJsonHelpers.h"

FMCPToolsetResult UMyProjectToolset::get_character_health(
    const FString& blueprint_path)
{
    // ... read from Blueprint defaults ...
    auto Response = MakeShared<FJsonObject>();
    Response->SetNumberField(TEXT("max_health"), 100.0);
    return FMCPToolsetResult::Success(
        MCPJsonHelpers::SerializeCompact(Response));
}

FMCPToolsetResult UMyProjectToolset::set_character_max_health(
    const FString& blueprint_path, float max_health)
{
    // ... modify Blueprint defaults ...
    auto Response = MakeShared<FJsonObject>();
    Response->SetStringField(TEXT("status"), TEXT("ok"));
    return FMCPToolsetResult::Success(
        MCPJsonHelpers::SerializeCompact(Response));
}
```

### Key Conventions

| Convention | Description |
| --- | --- |
| **Function name = tool name** | `get_character_health` is callable as `tools/call` with name `"get_character_health"` |
| **`@param` comments = schema** | Doxygen `@param` lines become the tool's parameter descriptions in the MCP schema |
| **Default values = optional params** | `bool verbose = false` makes the parameter optional with a default |
| **`meta=(Mutating)`** | Add to any tool that modifies state. The server wraps it in `FScopedTransaction` for undo support |
| **Return `FMCPToolsetResult`** | Use `::Success(json)` for results, `::Error(msg)` for failures |

### No Registration Needed

At startup, `FMCPToolsetAdapter::RegisterAllToolsets()` scans all `UMCPToolset` subclasses via Unreal's reflection system. Your toolset is discovered and registered automatically. Just add the class and rebuild.

### Tool & Parameter Aliases

You can add aliases so AI agents can call your tools with alternative names:

```cpp
UFUNCTION(BlueprintCallable, Category = "MyProject",
    meta = (Aliases = "get_char_hp",
            ParameterAlias_blueprint_path = "bp_path"))
static FMCPToolsetResult get_character_health(
    const FString& blueprint_path);
```

Now the tool responds to both `get_character_health` and `get_char_hp`, and accepts both `blueprint_path` and `bp_path` as parameter names.

## FAQ

**Q: What is MCP?**
MCP (Model Context Protocol) is an open standard by Anthropic that lets AI agents communicate with external tools over a structured JSON-RPC 2.0 interface. UnrealMCPCore implements an MCP server inside the Unreal Editor so any compatible AI client can connect and use its tools.

**Q: Which AI clients work with this plugin?**
Any client that speaks MCP over HTTP/HTTPS or stdio. Tested with Claude Desktop, Devin, and Cursor. The plugin includes an stdio proxy script for clients that require subprocess communication (e.g. Claude Desktop).

**Q: Does this plugin require Python or Node.js?**
No. The plugin is pure C++ and runs entirely inside the Unreal Editor. The only Python component is an optional stdio proxy script (stdlib only, no pip dependencies) for MCP clients that don't support direct HTTP connections.

**Q: Does it work at runtime / in packaged builds?**
No. UnrealMCPCore is an Editor-only plugin. It is designed for development workflows where AI agents assist with building, editing, and inspecting the project inside the Unreal Editor. It has no effect on packaged builds.

**Q: Which platforms are supported?**
Windows, macOS, and Linux. The plugin uses only cross-platform Unreal Engine APIs with no platform-specific code.

**Q: Can multiple AI agents connect at the same time?**
Yes. The server supports multiple concurrent sessions. Each client gets its own session ID, and all shared state is protected by locks. There is no limit on the number of simultaneous connections.

**Q: What happens if I don't have all engine plugins installed (e.g. GameplayAbilities, Niagara)?**
Nothing breaks. Tools that depend on optional plugins are isolated into separate extension DLLs. If a required plugin is missing, that extension module is silently skipped at startup. All other tools remain fully available.

**Q: How do I change the server port?**
Go to Project Settings > Plugins > MCP Core and change the **Server Port** setting. Then restart the MCP server via the Monitor Panel's Restart button or `MCP.Stop` / `MCP.Start [Port]` console commands. No editor restart needed.

**Q: How do I protect the server with an API key?**
Go to Project Settings > Plugins > MCP Core, enable **Require API Key**, and set your key. Clients must then include `X-API-Key: <key>` or `Authorization: Bearer <key>` in every request. The stdio proxy supports `--api-key <key>`. The health endpoint remains accessible without authentication.

**Q: How do I connect Claude Desktop?**
Click the **Client Config** button in the MCP Monitor Panel or in Project Settings > MCP Core. Select "stdio" transport, and copy the generated JSON snippet into your `claude_desktop_config.json`. The snippet includes the full path to the stdio proxy script.

**Q: Can I see what my AI agent is doing?**
Yes. The MCP Monitor Panel (bottom status bar) logs every tool call in real time with arguments, results, duration, and clickable asset links. You can revert (undo) or retry any operation from the context menu.

**Q: A tool returned an error. How do I report it?**
Click the **Report Issue** button in the MCP Monitor Panel. This automatically exports the request log to `Saved/MCPLogs/`, copies the file path to your clipboard, and opens a pre-filled GitHub issue form in your browser. Just describe the problem, drag-and-drop the log file into the issue, and submit. You can also email [f.wiegand00@web.de](mailto:f.wiegand00@web.de) with the exported log attached.

**Q: I need a tool that doesn't exist yet. Can I request it?**
Absolutely. Feature requests for new tools, new categories, or additional functionality in existing tools are welcome. Reach out via [f.wiegand00@web.de](mailto:f.wiegand00@web.de) or the Fab product page.
