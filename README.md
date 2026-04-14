# UnrealMCPCore

A pure C++ Unreal Engine 5 Editor plugin that exposes an MCP (Model Context Protocol) server over HTTP (default) or HTTPS, enabling AI clients like Devin to read, write, and interact with Blueprints, Assets, Levels, and more, all without Python or Node.js dependencies.

## Overview

UnrealMCPCore runs an HTTP server inside the Unreal Editor on `localhost:8090`, speaking JSON-RPC 2.0 over the MCP protocol. No manual setup required. Just open the project and connect. For production or remote use, enable HTTPS (TLS 1.2+) in settings; a self-signed certificate is generated automatically. AI agents connect to it and use registered tools to inspect and manipulate the project.

**Engine Version:** UE 5.5  
**Platform:** Win64, Mac, Linux  
**Module Type:** Editor  
**Loading Phase:** PostEngineInit

> **Epic Games MCP Standard:** UnrealMCPCore's architecture closely follows the official MCP integration that Epic Games is developing for Unreal Engine. Once Epic's built-in MCP support ships, this project will migrate to build on top of the official standard and extend it with the full tool set already available here. Existing users will benefit from a seamless transition with continued support for all current tools and workflows.

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

The plugin provides **360 tools** across 30 categories covering Blueprints, Assets, Levels, Materials, Widgets, Animation, PCG, Niagara, Sequencer, AI (Behavior Trees, Blackboards, State Trees, EQS, Smart Objects), Mesh, Enhanced Input, Gameplay Abilities, Audio/MetaSounds, Landscape, Physics, Foliage, World Partition, Control Rig, Rendering, Curves, Motion Design, Project/Editor Info, and PIE/Testing.

Each tool has full input/output documentation with parameter tables, response schemas, and usage examples.

**[View the complete Tool Reference](tools/README.md)**

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

**Q: Epic Games is building official MCP support. Will this plugin become obsolete?**
No. UnrealMCPCore's implementation closely follows the architecture of Epic's upcoming MCP standard. Once the official integration ships, this project will migrate to build on top of it, using the standard as a foundation and extending it with the full tool set already available here. Existing workflows and client configurations will continue to work.
