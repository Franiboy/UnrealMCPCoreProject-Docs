# get_blueprint

Get comprehensive information about a Blueprint asset including variables, functions (with UFunction flags and metadata via reflection), components (with reflection-based property diffs), event graphs, interfaces, event dispatchers, class defaults, and compilation status.

## Input

```json
{
  "asset_path": "/Game/Blueprints/BP_MyActor",
  "category": "Rendering"
}
```

| Parameter    | Required | Description                                                        |
| ------------ | -------- | ------------------------------------------------------------------ |
| `asset_path` | yes      | Asset path (e.g. `/Game/BP_MyActor`)                               |
| `category`   | no       | Filter `class_defaults` to this category only (e.g. `Replication`) |

## Output

| Field                   | Description                                                          |
| ----------------------- | -------------------------------------------------------------------- |
| `name`                  | Blueprint asset name                                                 |
| `asset_path`            | Full asset path                                                      |
| `blueprint_type`        | Normal, Const, MacroLibrary, Interface, LevelScript, FunctionLibrary |
| `compile_status`        | UpToDate, Dirty, Error, BeingCreated                                 |
| `parent_class`          | Parent class name (e.g. Actor)                                       |
| `parent_class_path`     | Full path to parent class                                            |
| `generated_class`       | Path to the generated C++ class                                      |
| `variables[]`           | Array of variable descriptors                                        |
| `functions[]`           | Array of function descriptors                                        |
| `event_dispatchers[]`   | Array of event dispatcher descriptors                                |
| `components[]`          | Array of component descriptors                                       |
| `graphs[]`              | Array of graph summaries                                             |
| `interfaces[]`          | Array of implemented interfaces                                      |
| `class_defaults[]`      | Array of editable CDO properties (filterable by category)            |
| `available_categories`  | All property categories found in this Blueprint                      |

## Variable Descriptor Fields

| Field                    | Type   | Description                                                 |
| ------------------------ | ------ | ----------------------------------------------------------- |
| `name`                   | string | Variable name                                               |
| `type`                   | string | Type (e.g. `float`, `FString`, `Array<int>`, `LinearColor`) |
| `category`               | string | Category grouping                                           |
| `default_value`          | string | Default value (if set)                                      |
| `is_instance_editable`   | bool   | Editable per instance in the level                          |
| `is_blueprint_read_only` | bool   | Read-only in Blueprints                                     |
| `is_expose_on_spawn`     | bool   | Exposed as a spawn parameter                                |
| `is_transient`           | bool   | Not saved to disk                                           |
| `is_save_game`           | bool   | Included in save games                                      |
| `replication`            | string | `None`, `Replicated`, or `RepNotify`                        |
| `tooltip`                | string | Tooltip text (if set)                                       |

## Function Descriptor Fields

| Field              | Type   | Description                                                                 |
| ------------------ | ------ | --------------------------------------------------------------------------- |
| `name`             | string | Function name                                                               |
| `return_type`      | string | Return type (e.g. `void`, `bool`, `FString`)                                |
| `is_pure`          | bool   | Pure function (no execution pin)                                            |
| `is_const`         | bool   | Const function                                                              |
| `is_static`        | bool   | Static function                                                             |
| `access`           | string | `Public`, `Protected`, or `Private`                                         |
| `category`         | string | Category (if set)                                                           |
| `tooltip`          | string | Tooltip (if set)                                                            |
| `parameters[]`     | array  | Parameters with `name`, `type`, `direction` (input/output), `default_value` |
| `function_flags[]` | array  | UFunction flags from the compiled function (e.g. `BlueprintCallable`, `BlueprintPure`, `Const`, `Public`). Only present when the Blueprint has a compiled `GeneratedClass`. |
| `metadata`         | object | All metadata key/value pairs from the compiled `UFunction` (auto-captured via reflection). Only present when metadata exists. |

## Component Descriptor Fields

| Field                | Type   | Description                                          |
| -------------------- | ------ | ---------------------------------------------------- |
| `name`               | string | Component variable name                              |
| `class`              | string | Component class (e.g. `StaticMeshComponent`)         |
| `is_root`            | bool   | Is the default scene root                            |
| `is_scene_component` | bool   | Has transform (is a scene component)                 |
| `parent`             | string | Parent component name (if any)                       |
| `relative_transform` | object | `location`, `rotation`, `scale` as formatted strings |
| `properties`         | object | Editable properties that differ from the component class default (via reflection). Booleans are native JSON `true`/`false`, numbers are native JSON numbers, enums are name strings, structs/objects use Unreal `ExportText` format. Only present when at least one property differs from the class CDO. |

## Graph Summary Fields

| Field              | Type   | Description                                    |
| ------------------ | ------ | ---------------------------------------------- |
| `name`             | string | Graph name                                     |
| `graph_type`       | string | `EventGraph`, `FunctionGraph`, or `MacroGraph` |
| `node_count`       | int    | Total number of nodes                          |
| `node_type_counts` | object | Map of node class name to count                |

## Interface Descriptor Fields

| Field         | Type   | Description                        |
| ------------- | ------ | ---------------------------------- |
| `name`        | string | Interface class name               |
| `path`        | string | Full asset path                    |
| `functions[]` | array  | Names of interface function graphs |
