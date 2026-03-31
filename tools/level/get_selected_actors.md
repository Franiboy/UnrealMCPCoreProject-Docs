# get_selected_actors

Get the currently selected actors in the editor viewport. Returns name, label, class, visibility, folder, and transform for each selected actor. Optionally includes tags, layers, component hierarchy, and reflection-based properties.

## Input

```json
{}
```

| Parameter            | Required | Default | Description                                              |
| -------------------- | -------- | ------- | -------------------------------------------------------- |
| `include_transform`  | no       | `true`  | Include transform (location, rotation, scale) per actor. |
| `include_tags`       | no       | `true`  | Include actor tags.                                      |
| `include_components` | no       | `false` | Include component hierarchy with names, classes, and transforms. |
| `include_properties` | no       | `false` | Include non-default reflection-based properties for actors and components. |

## Output

| Field    | Type   | Description                                       |
| -------- | ------ | ------------------------------------------------- |
| `count`  | number | Number of selected actors                         |
| `actors` | array  | Array of actor objects (see below)                |

### Actor object

| Field         | Type    | Description                                              |
| ------------- | ------- | -------------------------------------------------------- |
| `name`        | string  | Internal actor name                                      |
| `label`       | string  | Editor display label                                     |
| `class`       | string  | Class name (e.g. `StaticMeshActor`)                     |
| `class_path`  | string  | Full class path                                          |
| `is_hidden`   | boolean | Whether the actor is hidden                              |
| `folder`      | string  | Editor folder path (omitted if none)                     |
| `transform`   | object  | Location/rotation/scale (if `include_transform`)         |
| `tags`        | array   | Actor tags as strings (if `include_tags` and tags exist) |
| `layers`      | array   | Actor layers as strings (if layers exist)                |
| `components`  | array   | Component hierarchy (if `include_components`)            |
| `component_count` | number | Number of components (if `include_components`)       |
| `properties`  | object  | Non-default actor properties (if `include_properties`)   |

## Examples

### Get selected actors with defaults

```json
{}
```

### Get selected actors with components

```json
{
  "include_components": true
}
```

### Get selected actors with full detail

```json
{
  "include_components": true,
  "include_properties": true
}
```

### Minimal output (no transform or tags)

```json
{
  "include_transform": false,
  "include_tags": false
}
```

## Errors

| Condition                     | Error message                          |
| ----------------------------- | -------------------------------------- |
| Editor not available          | `Editor not available`                 |
| No selection context          | `No selection context available`       |

Returns `count: 0` with an empty `actors` array if no actors are selected (not an error).
