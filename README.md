# UnrealMCPCore

A pure C++ Unreal Engine 5 Editor plugin that exposes an MCP (Model Context Protocol) server over HTTP, enabling AI clients like Devin to read, write, and interact with Blueprints, Assets, Levels, and more — all without Python or Node.js dependencies.

## Overview

UnrealMCPCore runs an HTTP server inside the Unreal Editor on `localhost:8090`, speaking JSON-RPC 2.0 over the MCP protocol. AI agents connect to it and use registered tools to inspect and manipulate the project.

**Engine Version:** UE 5.5  
**Platform:** Win64, Mac, Linux  
**Module Type:** Editor  
**Loading Phase:** PostEngineInit

## Quick Start

1. Open the project in Unreal Editor — the MCP server starts automatically on port 8090
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
| **Server Port**       | `8090`  | The port the HTTP server listens on. Requires editor restart.                                        |
| **Auto Start Server** | `true`  | Automatically start the server when the editor loads.                                                |
| **Verbose Logging**   | `false` | Log all MCP requests and responses in detail. Toggles live — no restart needed.                      |
| **Max Log Entries**   | `1000`  | Maximum request log entries kept in memory.                                                          |
| **Enable CORS**       | `true`  | Add CORS headers to responses (required for browser-based clients).                                  |
| **CORS Origin**       | `*`     | Allowed origin for CORS (`*` = any).                                                                 |
| **Require API Key**   | `false` | Require an API key for all MCP requests. Clients must send `X-API-Key` or `Authorization: Bearer <key>`. The `/mcp/health` endpoint is always accessible without authentication. |
| **API Key**           | *(empty)* | The API key clients must provide. Displayed as a password field. Leave empty to disable auth regardless of the toggle. |
| **Enable Rate Limiting** | `false` | Throttle requests with a per-session token bucket. Returns HTTP 429 when exceeded.                |
| **Max Requests/Min**  | `60`    | Sustained token refill rate (tokens per minute). Only applies when rate limiting is enabled.          |
| **Burst Size**        | `10`    | Maximum burst — requests allowed before throttling starts. Only applies when rate limiting is enabled.|
| **Tool Timeout (s)**  | `30`    | Maximum tool execution time in seconds. Exceeded tools are flagged with `_timeout_exceeded`. Set to 0 to disable. |

Settings are stored in `Config/DefaultMCPCore.ini` and can be overridden per-platform.

### MCP Monitor Panel

The MCP Monitor is a **status bar drawer** — a button in the bottom status bar (next to Output Log and Content Drawer) that slides up a monitoring panel when clicked.

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

The panel refreshes automatically every 0.5 seconds. Only tool calls are shown in the log — protocol methods (`initialize`, `ping`, `tools/list`, `resources/list`) are handled silently.

### Client Config Helper

Click the **"Client Config"** button (available in the Monitor Panel header and in Project Settings > MCP Core) to open a popup dialog with the ready-to-paste JSON snippet for your MCP client. A transport dropdown lets you switch between **HTTP** and **stdio** — the snippet updates automatically with the current server port and the resolved path to the stdio proxy script.

## MCP Protocol

The server implements the [Model Context Protocol](https://modelcontextprotocol.io/) over Streamable HTTP transport:

- **Endpoint:** `POST http://localhost:8090/mcp` (JSON-RPC 2.0)
- **Discovery:** `GET http://localhost:8090/mcp` (returns SSE `event: endpoint`)
- **Health:** `GET http://localhost:8090/mcp/health`
- **Session Termination:** `DELETE http://localhost:8090/mcp`

### SSE (Server-Sent Events) Support

The server supports the **Streamable HTTP** transport defined by the MCP spec. Clients that send `Accept: text/event-stream` receive responses wrapped in SSE format (`event: message`, `data: {…}`). Clients that only send `Accept: application/json` (or no Accept header) receive plain JSON-RPC responses as before — full backward compatibility is preserved.

`GET /mcp` returns an SSE `event: endpoint` with `data: {"uri":"…"}` for transport discovery, as specified by the Streamable HTTP protocol.

> **Note:** UE5's built-in HTTP server is request/response only, so each SSE response is a single completed event — not a persistent stream. This covers all current MCP tool call semantics and lays the groundwork for future true-streaming support.

### stdio Transport (Proxy)

For MCP clients that only support **stdio** transport (spawning the server as a subprocess), a lightweight Python proxy script is included. It reads JSON-RPC from stdin, forwards to the HTTP server, and writes responses to stdout. No external dependencies — Python 3.7+ stdlib only.

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
| `initialize` | MCP handshake — creates or reuses a session |
| `notifications/initialized` | Client confirms initialization |
| `ping` | Health check (returns `{"status":"ok"}`) |
| `tools/list` | List all registered tools with input schemas |
| `tools/call` | Execute a tool by name with arguments |
| `resources/list` | Returns `{"resources":[]}` — no resources exposed, but handled cleanly for MCP client compatibility |

### Session & Auto-Initialization

The server supports **multiple concurrent sessions** — several AI agents (Devin, Claude Desktop, Cursor, etc.) can connect simultaneously, each with its own session. All shared state (sessions, tool registry, rate-limit buckets) is protected by `FCriticalSection` locks, so concurrent requests are safe. Tool execution itself runs outside any lock to avoid blocking other requests.

- **`initialize`** creates a new session and returns a unique session ID in the `Mcp-Session-Id` response header. Clients must include this header in subsequent requests.
- **Re-initialization** is idempotent — calling `initialize` with an existing session ID reuses that session.
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

350 tools available (41 Blueprint + 14 Asset + 14 Level + 17 Material + 9 DataTable/Struct + 23 Widget/UI + 28 Animation + 13 PCG + 11 Niagara + 13 Sequencer + 7 Behavior Tree + 6 Blackboard + 7 State Tree + 5 EQS + 5 Smart Object + 12 Mesh + 7 Enhanced Input + 13 GAS + 19 Audio + 9 Landscape + 12 Physics + 7 Foliage + 7 World Partition + 6 Control Rig + 6 Rendering Config + 6 Curve Asset + 5 Motion Design + 17 Project + 1 Infrastructure + 10 PIE / Testing). Full input/output documentation with examples: **[docs/tools/](tools/README.md)**

#### Blueprint Read Tools (7)

| Tool | Description |
| ---- | ----------- |
| `get_blueprint` | Full Blueprint introspection (variables, functions, components, graphs, class defaults) with category filtering |
| `list_blueprints` | List/search Blueprints via Asset Registry with path, type, and name filters |
| `compile_blueprint` | Single or batch compilation with error/warning collection |
| `get_blueprint_graph` | Node-level graph data: pins, connections, positions, types |
| `get_blueprint_node` | Single-node deep inspection with reflection-based properties bag |
| `search_blueprint_nodes` | Search nodes by title, class, or comment across all graphs |
| `get_blueprint_diff` | Compare two Blueprints structurally (variables, functions, components, graphs, defaults) |

#### Blueprint Write Tools (20)

| Tool | Description |
| ---- | ----------- |
| `create_blueprint` | Create a Blueprint with configurable parent class (C++ name, full path, or BP class) |
| `delete_blueprint` | Delete with reference checking and optional force mode |
| `rename_blueprint` | Rename/move a Blueprint with automatic redirector creation |
| `duplicate_blueprint` | Copy a Blueprint to a new name/path, preserving parent class |
| `reparent_blueprint` | Change parent class, recompile, return old/new parent info |
| `add_blueprint_variable` | Add a typed member variable with metadata (category, default, tooltip, visibility flags) |
| `remove_blueprint_variable` | Remove a member variable by name, returns removed type and metadata for undo |
| `set_blueprint_variable_properties` | Change variable type, default, category, tooltip, flags, replication, transient, save-game |
| `add_blueprint_function` | Create a function graph with typed inputs/outputs, return type, pure/const/access flags, category, tooltip |
| `add_blueprint_macro` | Create a macro graph with optional tunnel pins (supports exec pins for multi-output flow control) |
| `remove_blueprint_function` | Remove a function graph by name, returns full signature for undo |
| `remove_blueprint_macro` | Remove a macro graph by name, returns tunnel pin metadata for undo |
| `add_blueprint_component` | Add a component to the Blueprint's component tree with optional transform and parent attachment |
| `remove_blueprint_component` | Remove a component by name, returns class/parent/transform for undo |
| `set_component_property` | Set properties on a component template with nested dot-notation paths |
| `add_blueprint_interface` | Implement an interface on a Blueprint (C++ or Blueprint Interface) |
| `remove_blueprint_interface` | Remove an interface from a Blueprint, deleting associated function graphs |
| `add_event_dispatcher` | Add a multicast delegate (event dispatcher) with optional typed parameters |
| `remove_event_dispatcher` | Remove an event dispatcher by name, returns parameters for undo |
| `set_blueprint_defaults` | Modify CDO properties with nested dot-notation paths (e.g. `MyVector.X`) |

#### Graph / Node Tools (14)

| Tool | Description |
| ---- | ----------- |
| `add_node` | Add a node to a graph by K2Node class (function calls, custom events, variable get/set, flow control) |
| `remove_node` | Remove a node from a graph by node ID |
| `connect_pins` | Wire two pins together (validates type compatibility via graph schema) |
| `disconnect_pins` | Break a wire between two pins |
| `set_pin_default` | Set the default value on an input pin (string, bool, float, enum, struct literals) |
| `move_node` | Reposition a node in the graph editor (cosmetic, no recompile) |
| `add_comment` | Add a comment box to a Blueprint graph |
| `add_custom_event` | Add a custom event node with optional typed output parameters |
| `add_function_call` | Add a function call node (by ClassName::FunctionName or auto-discovery) |
| `add_variable_get` | Add a variable getter node (Blueprint or inherited property) |
| `add_variable_set` | Add a variable setter (Set) node to a graph |
| `collapse_to_function` | Collapse selected nodes into a new function |
| `collapse_to_macro` | Collapse selected nodes into a new macro |
| `promote_to_variable` | Promote a pin to a new member variable (getter or setter) |

#### Asset Tools (14)

| Tool | Description |
| ---- | ----------- |
| `list_assets` | List/search all asset types via Asset Registry with class, path, name filters and pagination |
| `get_asset_info` | Detailed asset metadata: class, size, tags, referencers, dependencies |
| `rename_asset` | Rename or move any asset type with automatic redirector creation |
| `move_asset` | Move any asset to a different content folder, keeping its name |
| `duplicate_asset` | Duplicate any asset with a new name and optional destination folder |
| `delete_asset` | Delete any asset with reference checking and optional force mode |
| `create_folder` | Create a content browser folder (with intermediate directories) |
| `find_references` | Find all assets referencing a given asset (direct or recursive) |
| `find_dependencies` | Find all assets a given asset depends on (direct or recursive) |
| `fix_redirectors` | Fix up and delete ObjectRedirectors in a content path |
| `import_asset` | Import external files (textures, meshes, audio, etc.) into the project |
| `export_asset` | Export an asset to an external file (PNG, FBX, WAV, etc.) |
| `save_asset` | Save a dirty asset's package to disk |
| `save_all` | Save all dirty asset packages to disk in one call |

#### Level / World Tools (14)

| Tool | Description |
| ---- | ----------- |
| `get_level_info` | Level name, bounds, streaming levels, actor count by class |
| `list_actors` | List actors in the current level with class, tag, folder, and name filters |
| `get_actor_info` | Detailed actor inspection: properties, components, transform, tags, layers |
| `spawn_actor` | Spawn an actor from a C++ class or Blueprint asset with transform, label, folder, and tags |
| `delete_actor` | Remove an actor from the level by name or label |
| `set_actor_transform` | Move, rotate, and/or scale an actor with partial field updates |
| `set_actor_property` | Set property values on an actor instance via reflection (dot-notation, arrays) |
| `get_world_settings` | World settings: game mode, physics/gravity, kill Z, rendering, lightmass, tick |
| `set_world_settings` | Modify world settings: physics, rendering, lightmass, tick via section.key paths |
| `get_streaming_levels` | List streaming/sub-levels with state, priority, blocking flags, transform, actor count |
| `load_level` | Open a different level/map in the editor by path or name |
| `get_selected_actors` | Get currently selected actors in the editor viewport with optional detail |
| `select_actors` | Select actors by name, class, tag, folder, or search filter with visual feedback |
| `focus_actor` | Focus the viewport camera on an actor, framing its bounding box |

#### Material Tools (17)

| Tool | Description |
| ---- | ----------- |
| `get_material_info` | Inspect material properties, parameters, expressions, domain, blend mode, shading model |
| `list_material_parameters` | List all parameters of a material/instance by type with values and override status |
| `create_material` | Create a new material asset with domain, blend mode, shading model, two-sided flag |
| `create_material_instance` | Create a material instance from a parent with optional scalar, vector, texture parameter overrides |
| `set_material_parameter` | Set a scalar, vector, or texture parameter on a material instance |
| `assign_material` | Assign a material to a mesh component slot on an actor in the current level |
| `set_material_properties` | Modify base material properties (domain, blend mode, shading model, two-sided, etc.) |
| `compile_material` | Force recompile a material and return errors |
| `get_material_slots` | Read material slot assignments from an actor's mesh components |
| `set_static_switch_parameter` | Set a static switch parameter on a material instance (shader permutation) |
| `clear_parameter_override` | Clear/reset a parameter override on a material instance (revert to parent) |
| `set_material_instance_parent` | Change the parent material of an existing material instance |
| `add_material_expression` | Add an expression node to a material graph by class name |
| `remove_material_expression` | Remove an expression node from a material graph by GUID or index |
| `set_material_expression_property` | Set properties on a material expression node via reflection |
| `connect_material_expression` | Wire expression outputs to material pins or other expression inputs |
| `disconnect_material_expression` | Break connections at material pins or expression inputs |

#### DataTable / Struct Tools (9)

| Tool | Description |
| ---- | ----------- |
| `create_data_table` | Create a new DataTable asset with a specified row struct |
| `list_data_tables` | List DataTable assets with their row struct, row count, and optional filtering |
| `get_data_table_rows` | Get all rows (or filtered subset) from a DataTable with field values |
| `add_data_table_row` | Add a new row to a DataTable with field values |
| `modify_data_table_row` | Modify field values of an existing DataTable row |
| `delete_data_table_row` | Remove a row from a DataTable by name |
| `create_user_defined_struct` | Create a new UserDefinedStruct asset with initial fields |
| `modify_user_defined_struct` | Add, remove, rename, or change field types in a UserDefinedStruct |
| `get_struct_info` | Get fields, types, and default values of any UStruct |

#### Widget / UI Tools (23)

| Tool | Description |
| ---- | ----------- |
| `create_widget_blueprint` | Create a new UMG Widget Blueprint asset |
| `list_widget_blueprints` | List Widget Blueprints with path and name filtering |
| `list_widget_types` | List available widget classes for adding to Widget Blueprints |
| `get_widget_tree` | Get the full widget hierarchy of a Widget Blueprint |
| `get_widget_properties` | Get detailed properties of a specific widget |
| `add_widget` | Add a child widget to a panel |
| `remove_widget` | Remove a widget from the hierarchy |
| `set_widget_property` | Modify widget properties (text, color, visibility, etc.) |
| `set_widget_slot` | Modify slot/layout properties (anchors, offsets, padding, etc.) |
| `reparent_widget` | Move a widget to a different parent panel |
| `get_widget_animations` | List all animations in a Widget Blueprint (name, duration, tracks) |
| `create_widget_animation` | Create a widget animation within a Widget Blueprint |
| `add_animation_track` | Add a track to a widget animation (animate opacity, position, color, scale, etc.) |
| `delete_widget_animation` | Delete an animation from a Widget Blueprint |
| `get_widget_event_bindings` | Get all event bindings for a widget (OnClicked, OnHovered, etc.) |
| `add_event_binding` | Bind a widget event to a Blueprint function |
| `remove_event_binding` | Remove an event binding from a widget |
| `remove_animation_track` | Remove a property track from a widget animation |
| `get_animation_keyframes` | Get all keyframes of a property track (time, value, interpolation) |
| `add_animation_keyframe` | Add or update a keyframe on a property track (value, interpolation) |
| `remove_animation_keyframe` | Remove a keyframe from a property track at a specific time |
| `set_widget_animation_length` | Change the duration of a widget animation |
| `rename_widget_animation` | Rename a widget animation |

#### Animation Tools (28)

| Tool | Description |
| ---- | ----------- |
| `list_animation_assets` | List AnimSequences, AnimMontages, BlendSpaces, and AnimBPs with filtering by type, skeleton, path, and name |
| `get_animation_info` | Detailed inspection of a single animation asset (duration, frames, notifies, curves, skeleton, sections, axes, samples) |
| `get_anim_blueprint_info` | Deep AnimBP inspection: state machines (states, transitions, entry state), variables, event/function graphs |
| `get_skeleton_info` | Skeleton bone hierarchy, sockets, virtual bones, animation slots, curves, blend profiles, compatible skeletons |
| `create_anim_montage` | Create an AnimMontage from a skeleton with optional source AnimSequence and slot configuration |
| `create_blend_space` | Create a BlendSpace (1D or 2D) with axis settings and target skeleton |
| `create_anim_blueprint` | Create an Animation Blueprint for a target skeleton with optional parent class |
| `add_anim_notify` | Add a notify event to an AnimSequence or AnimMontage (custom or typed class, with optional duration) |
| `remove_anim_notify` | Remove a notify event by name or index from an AnimSequence or AnimMontage |
| `add_anim_curve` | Add a float curve to an AnimSequence (empty; populate with set_anim_curve_keys) |
| `remove_anim_curve` | Remove a float curve from an AnimSequence |
| `set_anim_curve_keys` | Set keyframes on a float curve with time, value, and interpolation mode |
| `set_anim_montage_sections` | Add, remove, or link montage sections (next-section chaining) |
| `set_anim_montage_slot` | Change the slot name on a montage track |
| `set_blend_space_axis` | Configure BlendSpace axis parameters (name, min/max range, grid divisions, snap, wrap) |
| `add_blend_space_sample` | Add an AnimSequence sample to a BlendSpace at a position (X, Y) |
| `remove_blend_space_sample` | Remove a sample from a BlendSpace by index or animation reference |
| `add_anim_state` | Add a state to a state machine in an Animation Blueprint |
| `remove_anim_state` | Remove a state (and its transitions) from a state machine |
| `add_anim_transition` | Add a transition between two states with crossfade, priority, and bidirectional settings |
| `remove_anim_transition` | Remove a transition between two states by source and target name |
| `set_anim_state_animation` | Set the animation played by a state (creates/updates SequencePlayer node) |
| `add_anim_bp_variable` | Add a variable to an Animation Blueprint (bool, int, float, vector, etc.) |
| `set_anim_bp_variable` | Modify variable properties (default value, category, editability, read-only) |
| `add_skeleton_socket` | Add a socket to a skeleton bone (name, transform offset) |
| `remove_skeleton_socket` | Remove a socket from a skeleton by name |
| `add_virtual_bone` | Add a virtual bone between a source and target bone |
| `remove_virtual_bone` | Remove a virtual bone by name |

#### PCG Query Tools (4)

| Tool | Description |
| ---- | ----------- |
| `list_pcg_graphs` | List PCG graph assets with path, name, node count, and generation settings |
| `get_pcg_graph_info` | Detailed PCG graph inspection: nodes, pins, edges, hierarchical generation |
| `get_pcg_node` | Deep single-node inspection: settings, seed, pins with connections, overridable params |
| `get_pcg_component_info` | Inspect PCG component on an actor: graph reference, generation status, grid size |

#### PCG Lifecycle Tools (2)

| Tool | Description |
| ---- | ----------- |
| `create_pcg_graph` | Create a new PCG Graph asset (saved to disk, registered with Asset Registry) |
| `add_pcg_component` | Add/update a PCG Component on an actor and optionally assign a graph |

#### PCG Graph Editing Tools (5)

| Tool | Description |
| ---- | ----------- |
| `add_pcg_node` | Add a node to a PCG Graph by settings class name (e.g. SurfaceSampler, StaticMeshSpawner) |
| `remove_pcg_node` | Remove a node from a PCG Graph |
| `connect_pcg_nodes` | Wire two PCG nodes together (output pin to input pin) |
| `disconnect_pcg_nodes` | Break a wire between two PCG nodes |
| `set_pcg_node_settings` | Set settings/properties on a PCG node via reflection |

#### PCG Execution Tools (2)

| Tool | Description |
| ---- | ----------- |
| `execute_pcg_graph` | Trigger PCG graph generation on an actor's PCG component (async) |
| `clear_pcg_generation` | Clear/cleanup all generated content from a PCG component |

#### Niagara Query Tools (3)

| Tool | Description |
| ---- | ----------- |
| `list_niagara_assets` | List Niagara System and Emitter assets (filter by path, name, type) |
| `get_niagara_system_info` | Inspect a Niagara System: emitters, exposed parameters, bounds, warmup |
| `get_niagara_emitter_info` | Inspect a Niagara Emitter: renderers, sim target, scripts, simulation stages |

#### Niagara Lifecycle Tools (2)

| Tool | Description |
| ---- | ----------- |
| `create_niagara_system` | Create a new Niagara System asset (empty or from a template system) |
| `create_niagara_emitter` | Create a new Niagara Emitter asset (empty or from a template emitter) |

#### Niagara Editing Tools (6)

| Tool | Description |
| ---- | ----------- |
| `add_niagara_emitter_to_system` | Add an emitter reference to a Niagara System |
| `remove_niagara_emitter_from_system` | Remove an emitter from a Niagara System by name |
| `get_niagara_parameters` | List all user-exposed parameters with types and current values |
| `set_niagara_parameter` | Set a user-exposed parameter (float, int, bool, vector, color) |
| `set_niagara_emitter_property` | Set emitter properties (sim target, bounds, allocation, etc.) |
| `set_niagara_renderer` | Configure renderer settings (enabled, material, UPROPERTY values) |

#### Sequencer Tools (13)

| Tool | Description | Key Parameters |
| ---- | ----------- | -------------- |
| `list_level_sequences` | List Level Sequence assets | `path_prefix`, `name_filter`, `include_engine` |
| `get_level_sequence_info` | Inspect a Level Sequence | `asset_path`, `include_bindings`, `include_tracks` |
| `get_sequence_track` | Detailed track inspection | `asset_path`, `binding_name`, `track_index`, `include_keyframes`, `max_keyframes` |
| `create_level_sequence` | Create a new Level Sequence asset | `name`, `path`, `frame_rate`, `duration` |
| `add_sequence_binding` | Bind an actor to a Level Sequence | `asset_path`, `actor_name`, `binding_type` |
| `remove_sequence_binding` | Remove a binding from a Level Sequence | `asset_path`, `binding_name` |
| `add_sequence_track` | Add a track to a binding or master | `asset_path`, `binding_name`, `track_type` |
| `remove_sequence_track` | Remove a track from a sequence | `asset_path`, `binding_name`, `track_index` |
| `add_sequence_section` | Add a section to a track | `asset_path`, `track_index`, `binding_name`, `start_time`, `end_time` |
| `remove_sequence_section` | Remove a section from a track | `asset_path`, `track_index`, `section_index`, `binding_name` |
| `add_sequence_keyframe` | Add/set a keyframe on a channel | `asset_path`, `track_index`, `time`, `value`, `channel_index`, `interpolation` |
| `remove_sequence_keyframe` | Remove a keyframe from a channel | `asset_path`, `track_index`, `time`, `channel_index` |
| `set_sequence_playback` | Set playback range, frame rate, view/working range | `asset_path`, `start_time`, `end_time`, `frame_rate` |

#### Behavior Tree Tools (7)

| Tool | Description |
| ---- | ----------- |
| `list_behavior_trees` | List Behavior Tree assets with path and name filtering |
| `get_behavior_tree_info` | Inspect a BT: root node hierarchy, tasks, decorators, services, blackboard |
| `create_behavior_tree` | Create a new BT asset with root composite (Selector, Sequence, SimpleParallel) |
| `add_bt_node` | Add a node to a BT (Composite, Task, Decorator, Service) by path |
| `remove_bt_node` | Remove a node from a BT by path |
| `set_bt_node_property` | Set a property on a BT node via reflection |
| `connect_bt_nodes` | Move/re-parent a BT node to a new parent composite |

#### Blackboard Tools (6)

| Tool | Description |
| ---- | ----------- |
| `list_blackboards` | List Blackboard Data assets with path and name filtering |
| `get_blackboard_info` | Inspect a Blackboard: keys (name, type, instance synced, description), parent chain |
| `create_blackboard` | Create a new Blackboard Data asset with optional parent for key inheritance |
| `add_blackboard_key` | Add a key (Bool, Int, Float, String, Name, Vector, Rotator, Enum, Object, Class) |
| `remove_blackboard_key` | Remove a key from a Blackboard by name |
| `set_blackboard_key_properties` | Modify key properties (instance synced, description, category) |

#### State Tree Tools (7)

| Tool | Description |
| ---- | ----------- |
| `list_state_trees` | List State Tree assets with path and name filtering |
| `get_state_tree_info` | Inspect a State Tree: states, transitions, tasks, conditions, evaluators |
| `create_state_tree` | Create a new State Tree asset with configurable schema |
| `add_state_tree_state` | Add a state (State, Group, Linked, Subtree) as root or child |
| `remove_state_tree_state` | Remove a state and its children from a State Tree |
| `add_state_tree_transition` | Add a transition between states (trigger, priority, delay) |
| `set_state_tree_task` | Set/configure a task on a state (class, properties, enabled) |

#### EQS Tools (5)

| Tool | Description |
| ---- | ----------- |
| `list_eqs_queries` | List EQS Query template assets with path and name filtering |
| `get_eqs_query_info` | Inspect an EQS Query: generators, tests, options, weighting |
| `create_eqs_query` | Create a new EQS Query template asset |
| `add_eqs_generator` | Add a generator to an EQS Query (Points Around, Actors Of Class, Grid, etc.) |
| `add_eqs_test` | Add a test to an EQS Query (Distance, Trace, Dot, PathFinding, etc.) |

#### Smart Object Tools (5)

| Tool | Description |
| ---- | ----------- |
| `list_smart_object_definitions` | List Smart Object Definition assets with path and name filtering |
| `get_smart_object_definition_info` | Inspect a Smart Object Definition: slots, offsets, tags, policies |
| `create_smart_object_definition` | Create a new Smart Object Definition asset |
| `add_smart_object_slot` | Add a slot to a Smart Object Definition (offset, rotation, tags, enabled) |
| `remove_smart_object_slot` | Remove a slot from a Smart Object Definition by index |

#### Static Mesh Tools (6)

| Tool | Description |
| ---- | ----------- |
| `get_static_mesh_info` | Inspect a Static Mesh: LODs, materials, sockets, bounds, Nanite, collision, lightmap |
| `set_static_mesh_property` | Set Static Mesh properties (lightmap_resolution, nanite_enabled, lod_group, min_lod) |
| `add_static_mesh_socket` | Add a socket to a Static Mesh |
| `remove_static_mesh_socket` | Remove a socket from a Static Mesh by name |
| `set_static_mesh_collision` | Configure collision settings (complexity, LOD for collision) |
| `set_static_mesh_material` | Assign a material to a Static Mesh material slot |

#### Skeletal Mesh Tools (3)

| Tool | Description |
| ---- | ----------- |
| `get_skeletal_mesh_info` | Inspect a Skeletal Mesh: LODs, materials, skeleton, physics asset, morph targets, bounds, sockets |
| `set_skeletal_mesh_property` | Set Skeletal Mesh properties (physics_asset, min_lod) |
| `set_skeletal_mesh_material` | Assign a material to a Skeletal Mesh material slot |

#### Mesh Operations (3)

| Tool | Description |
| ---- | ----------- |
| `merge_meshes` | Merge multiple Static Mesh actors into a single Static Mesh asset |
| `generate_lods` | Auto-generate LODs for a Static Mesh or Skeletal Mesh |
| `generate_collision` | Auto-generate collision for a Static Mesh (box, sphere, capsule, convex) |

#### Enhanced Input Tools (7)

| Tool | Description |
| ---- | ----------- |
| `list_input_mapping_contexts` | List Input Mapping Context assets with mapping counts |
| `get_input_mapping_context_info` | Inspect an IMC: action mappings, keys, triggers, modifiers |
| `create_input_action` | Create a new Input Action asset (Boolean, Axis1D, Axis2D, Axis3D) |
| `create_input_mapping_context` | Create a new Input Mapping Context asset |
| `add_input_mapping` | Add an action mapping to a context (action, key, triggers, modifiers) |
| `remove_input_mapping` | Remove an action mapping from a context by index |
| `set_input_action_property` | Set Input Action properties (value type, description, consume input, triggers, modifiers) |

#### GAS — Abilities & Effects (13)

| Tool | Description |
| ---- | ----------- |
| `list_gameplay_abilities` | List Gameplay Ability Blueprint assets: tags, instancing policy, net execution policy |
| `get_gameplay_ability_info` | Inspect a Gameplay Ability: policies, all tag containers, cost/cooldown effects |
| `list_gameplay_effects` | List Gameplay Effect Blueprint assets: duration, modifiers, stacking, tags |
| `get_gameplay_effect_info` | Inspect a Gameplay Effect: modifiers, duration, stacking, tags, cues, components |
| `create_gameplay_ability` | Create a new Gameplay Ability Blueprint (custom parent class supported) |
| `create_gameplay_effect` | Create a new Gameplay Effect Blueprint (Instant/Infinite/HasDuration) |
| `set_gameplay_effect_modifier` | Add or edit a modifier on a Gameplay Effect (attribute, op, magnitude) |
| `list_attribute_sets` | List Attribute Set classes (C++ and Blueprint): name, origin, attribute count |
| `get_attribute_set_info` | Inspect an Attribute Set: attributes with name, base value, current value |
| `create_attribute_set` | Create a new Attribute Set Blueprint with initial FGameplayAttributeData attributes |
| `add_gameplay_tag` | Add a new Gameplay Tag to the project INI (idempotent if tag exists) |
| `remove_gameplay_tag` | Remove a Gameplay Tag from the project INI |
| `rename_gameplay_tag` | Rename a Gameplay Tag in the project INI (creates a redirector) |

#### Audio / MetaSounds (19)

| Tool | Description |
| ---- | ----------- |
| `list_audio_assets` | List Sound Waves, Sound Cues, MetaSounds, Sound Classes, Sound Mixes, Attenuation Settings |
| `get_sound_cue_info` | Inspect a Sound Cue: nodes, connections, volume/pitch, attenuation, sound class, duration |
| `get_metasound_info` | Inspect a MetaSound Source: metadata, inputs, outputs, graph pages, nodes, edges, dependencies |
| `create_sound_cue` | Create a new Sound Cue asset (optional volume/pitch multipliers) |
| `create_metasound` | Create a new MetaSound Source asset with default graph |
| `create_sound_class` | Create a new Sound Class asset (optional volume/pitch) |
| `create_sound_mix` | Create a new Sound Mix asset |
| `create_sound_attenuation` | Create a new Sound Attenuation Settings asset (optional shape, radius, falloff) |
| `add_sound_cue_node` | Add a node to a Sound Cue graph (WavePlayer, Mixer, Random, Modulator, Delay, etc.) |
| `remove_sound_cue_node` | Remove a node from a Sound Cue graph by index |
| `connect_sound_cue_nodes` | Wire two Sound Cue nodes together (parent -> child) |
| `set_sound_cue_node_property` | Set type-specific properties on a Sound Cue node |
| `add_metasound_node` | Add a node to a MetaSound graph by class name (e.g. UE.Add.Float) |
| `remove_metasound_node` | Remove a node from a MetaSound graph by GUID |
| `connect_metasound_nodes` | Wire two MetaSound nodes together (output -> input) |
| `set_metasound_input` | Set the default value of a MetaSound node input |
| `set_sound_class_properties` | Set Sound Class properties (volume, pitch, LPF, attenuation scale, flags) |
| `set_sound_mix_properties` | Set Sound Mix properties (EQ, timing, sound class effect overrides) |
| `set_sound_attenuation` | Set Sound Attenuation settings (shape, distance, spatialization, occlusion) |

#### Landscape / Terrain (9)

| Tool | Description |
| ---- | ----------- |
| `get_landscape_info` | Inspect a Landscape actor: size, components, layers, material, LOD, collision, Nanite, edit layers |
| `list_landscape_layers` | List Landscape Layer Info assets: layer name, hardness, physical material, weight-blend mode |
| `create_landscape` | Create a new flat Landscape actor with configurable grid size, section layout, position, and scale |
| `create_landscape_layer_info` | Create a new Landscape Layer Info asset for weight-based painting layers |
| `set_landscape_material` | Assign a material (and optional hole material) to a Landscape actor |
| `set_landscape_property` | Set Landscape properties (LOD, collision, navigation, Nanite, streaming) |
| `export_landscape_heightmap` | Export the Landscape heightmap to a raw uint16 binary file |
| `import_landscape_heightmap` | Import a raw uint16 heightmap file onto an existing Landscape |
| `import_landscape_layer_weight` | Import a raw uint8 weight map for a paint layer |

#### Physics Material Tools (4)

| Tool | Description |
| ---- | ----------- |
| `list_physics_materials` | List Physical Material assets: friction, restitution, density, surface type |
| `get_physics_material_info` | Inspect a Physical Material: all properties, combine modes, sleep thresholds, strength |
| `create_physics_material` | Create a new Physical Material asset with optional initial properties |
| `set_physics_material_property` | Set Physical Material properties (friction, restitution, density, surface type, strength, etc.) |

#### Physics Asset Tools (8)

| Tool | Description |
| ---- | ----------- |
| `get_physics_asset_info` | Inspect a Physics Asset: bodies (bone, shape, mass, physics type), constraints (limits, motion types) |
| `create_physics_asset` | Create a new empty Physics Asset |
| `add_physics_body` | Add a physics body with collision shape (sphere, box, capsule) to a Physics Asset |
| `remove_physics_body` | Remove a physics body by bone name or index (cascades constraint removal) |
| `set_physics_body_property` | Set body properties (mass, damping, physics type, simulate, gravity, collision profile) |
| `add_physics_constraint` | Add a constraint between two bodies with swing/twist limits |
| `remove_physics_constraint` | Remove a constraint from a Physics Asset by index |
| `set_physics_constraint_property` | Set constraint properties (motion types, limits, stiffness, damping, collision) |

#### Foliage Tools (7)

| Tool | Description |
| ---- | ----------- |
| `list_foliage_types` | List Foliage Type assets with optional path/name filtering and summary info |
| `get_foliage_type_info` | Detailed Foliage Type inspection: mesh, painting, placement, instance settings, scalability |
| `get_foliage_instances` | Query foliage instances in the current level with optional type and bounding box filters |
| `create_foliage_type` | Create a new Foliage Type (Instanced Static Mesh) from a Static Mesh with optional properties |
| `set_foliage_type_property` | Set Foliage Type properties: density, scaling, placement, shadows, culling, mobility |
| `add_foliage_instances` | Programmatically place foliage instances with location, rotation, and scale |
| `remove_foliage_instances` | Remove foliage instances by type, bounding box area, or all |

#### World Partition Tools (7)

| Tool | Description |
| ---- | ----------- |
| `get_world_partition_info` | Inspect World Partition settings: enabled state, streaming, grid info, HLOD defaults, data layer summary |
| `list_data_layers` | List all Data Layers with name, type, initial runtime state, visibility, and hierarchy |
| `get_hlod_info` | Get HLOD settings: default layer, all HLOD layer assets with configuration |
| `create_data_layer` | Create a new Data Layer asset and register it with the world's data layer system |
| `set_data_layer_state` | Set Data Layer state: editor visibility, loaded-in-editor, initial runtime state |
| `set_actor_data_layer` | Add or remove an actor from a Data Layer |
| `set_world_partition_settings` | Configure World Partition settings: streaming, default HLOD layer |

#### Control Rig Tools (6)

| Tool | Description |
| ---- | ----------- |
| `list_control_rigs` | List Control Rig Blueprint assets with optional name/path filtering |
| `get_control_rig_info` | Inspect hierarchy elements (bones, controls, nulls, curves), transforms, and control settings |
| `create_control_rig` | Create a new Control Rig Blueprint, optionally from a Skeleton |
| `add_control_rig_element` | Add an element (Bone, Control, Null, Curve) to a Control Rig's hierarchy |
| `remove_control_rig_element` | Remove an element from a Control Rig's hierarchy |
| `set_control_rig_element_property` | Set properties on a hierarchy element: transform, parent, shape, color |

#### Rendering Configuration Tools (6)

| Tool | Description |
| ---- | ----------- |
| `get_rendering_settings` | Get global rendering settings (GI, Reflections, Shadows, AA, Nanite, Ray Tracing) |
| `set_rendering_settings` | Modify global rendering settings via console variables |
| `get_post_process_settings` | Get Post Process Volume settings (bloom, exposure, DOF, etc.) |
| `set_post_process_settings` | Set Post Process Volume settings with auto-override flags |
| `set_nanite_settings` | Configure Nanite per-mesh or global settings |
| `set_lumen_settings` | Configure Lumen GI and reflection settings via CVars |

#### Curve Asset Tools (6)

| Tool | Description |
| ---- | ----------- |
| `list_curve_assets` | List Curve Float, Curve Vector, Curve Linear Color, and Curve Table assets |
| `get_curve_asset_info` | Inspect a Curve asset: keys, time/value ranges, type, optional evaluation |
| `create_curve_asset` | Create a new Curve asset (Float, Vector, or LinearColor) with optional initial keys |
| `set_curve_keys` | Set keyframes on a Curve asset (replace all or add/update individual keys) |
| `create_curve_table` | Create a new empty Curve Table asset |
| `add_curve_table_row` | Add a named row (curve) to a Curve Table with optional initial keys |

#### Motion Design Tools (5)

| Tool | Description |
| ---- | ----------- |
| `get_motion_design_info` | Inspect Cloners and Effectors, or list all Motion Design actors in the level |
| `create_motion_design_actor` | Spawn a Cloner or Effector actor with optional layout, seed, and transform |
| `set_motion_design_property` | Set properties on a Cloner or Effector (enabled, seed, layout, magnitude, etc.) |
| `add_motion_design_effector` | Link an Effector to a Cloner so it affects the cloner's instances |
| `add_motion_design_modifier` | Set effector shape/type and mode on an Effector actor |

#### Project / Editor Info Tools (17)

| Tool | Description |
| ---- | ----------- |
| `get_project_info` | Project name, engine version, target platforms, enabled plugins, maps, project modules |
| `list_plugins` | List all discovered plugins with version, description, category, type, and enabled status |
| `list_modules` | List all modules with loaded status, game module flag, and file path |
| `get_editor_state` | PIE status, loaded level, selected actors, editor mode, viewport camera, dirty packages |
| `get_editor_preferences` | Read editor INI config settings by section and key |
| `set_editor_preferences` | Write editor INI config settings by section and key |
| `get_class_hierarchy` | Inheritance chain for any UClass (from given class up to UObject) |
| `get_class_info` | Properties and functions of a native or Blueprint class (name, type, flags, category) |
| `get_enum_values` | Values and display names of a UEnum |
| `list_asset_types` | List all registered UClass types that can exist as assets |
| `get_project_settings` | Read project configuration from Default*.ini files (DefaultEngine, DefaultGame, etc.) |
| `set_project_settings` | Write/modify project configuration in Default*.ini files |
| `get_collision_profiles` | List collision presets and channels with object types and responses |
| `list_gameplay_tags` | List all gameplay tags defined in the project with hierarchy info |
| `list_input_actions` | List Enhanced Input Actions and Input Mapping Contexts with key bindings |
| `execute_console_command` | Run an Unreal console command and capture the output |
| `get_log_output` | Retrieve recent Output Log entries with category and verbosity filtering |

#### Infrastructure Tools (1)

| Tool | Description |
| ---- | ----------- |
| `get_request_log` | Query history of all MCP tool calls with filtering, limits, and optional full detail |

#### PIE / Testing Tools (10)

| Tool | Description |
| ---- | ----------- |
| `start_pie` | Start a Play In Editor session (viewport, new window, or simulate mode) |
| `stop_pie` | Stop the current PIE session |
| `is_pie_running` | Check PIE session state, paused status, elapsed time, player info |
| `pause_pie` | Pause or unpause the current PIE session |
| `take_screenshot` | Capture a screenshot of the editor or PIE viewport |
| `inject_input` | Simulate keyboard/mouse input or execute console commands in PIE |
| `get_fps` | Get frame rate and frame time statistics |
| `get_viewport_info` | Get editor viewport camera position, rotation, FOV, resolution |
| `set_viewport_camera` | Move the editor viewport camera position, rotation, and FOV |
| `run_automation_test` | List available automation tests or run a specific test by name |

---

## Architecture

```
+---------------------------------------------------+
|                 Unreal Editor                      |
|                                                    |
|  +---------------+   +-------------------------+  |
|  | FMCPHttpServer |-->| FMCPProtocolHandler     |  |
|  |  (port 8090)  |   |  - JSON-RPC 2.0         |  |
|  |  - POST /mcp  |   |  - Session management   |  |
|  |  - GET health |   |  - Tool registry        |  |
|  +---------------+   |  - Tool dispatch         |  |
|                       +-----+---+---+-----------+  |
|                             |   |   |              |
|       +---------------------+   |   +------+       |
|       v                         v          v       |
|  Core Tools (262)       Extension Modules (86)     |
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

Domain-specific tool modules (PCG, Niagara, StateTree, SmartObjects, GAS, ControlRig, MetaSounds, EnhancedInput, MotionDesign) are isolated into **separate extension DLLs** — one per optional dependency. The main plugin DLL (`UnrealMCPCore.dll`) has **zero optional plugin dependencies** and always loads successfully.

At startup, the main module attempts to load each extension module via `FModuleManager::LoadModule()`. If an extension's dependency plugin is not installed (e.g. a Fab user without GameplayAbilities), that extension DLL simply fails to load and the main module continues — all other tools remain available. This is critical for Fab distribution where users may not have all plugins installed.

| Extension Module | Plugin Required | Tools Provided |
| --- | --- | --- |
| `UnrealMCPCorePCG` | PCG | 13 PCG tools |
| `UnrealMCPCoreNiagara` | Niagara | 11 Niagara tools |
| `UnrealMCPCoreStateTree` | StateTree | 7 State Tree tools |
| `UnrealMCPCoreSmartObject` | SmartObjects | 5 Smart Object tools |
| `UnrealMCPCoreEnhancedInput` | EnhancedInput | 7 Enhanced Input tools |
| `UnrealMCPCoreGAS` | GameplayAbilities | 13 GAS tools |
| `UnrealMCPCoreMetaSound` | Metasound | 19 Audio/MetaSound tools |
| `UnrealMCPCoreControlRig` | ControlRig | 6 Control Rig tools |
| `UnrealMCPCoreMotionDesign` | ClonerEffector | 5 Motion Design tools |

See [README.md](https://github.com/Franiboy/UnrealMCPCoreProject/blob/master/README.md) for the full technical details on the extension module architecture.

For the full feature roadmap and checklist, see **[README_TODO.md](https://github.com/Franiboy/UnrealMCPCoreProject/blob/master/README_TODO.md)**.

## FAQ

**Q: What is MCP?**
MCP (Model Context Protocol) is an open standard by Anthropic that lets AI agents communicate with external tools over a structured JSON-RPC 2.0 interface. UnrealMCPCore implements an MCP server inside the Unreal Editor so any compatible AI client can connect and use its tools.

**Q: Which AI clients work with this plugin?**
Any client that speaks MCP over HTTP or stdio. Tested with Claude Desktop, Devin, and Cursor. The plugin includes an stdio proxy script for clients that require subprocess communication (e.g. Claude Desktop).

**Q: Does this plugin require Python or Node.js?**
No. The plugin is pure C++ and runs entirely inside the Unreal Editor. The only Python component is an optional stdio proxy script (stdlib only, no pip dependencies) for MCP clients that don't support HTTP.

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
