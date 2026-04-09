# search_blueprint_defaults

Search class default property values across multiple Blueprints. Finds properties by name, C++ type, or value substring. Ideal for locating which Blueprint sets a specific asset reference (e.g. a sound, texture, or mesh) without knowing which Blueprint contains it. Only loads Blueprints matching the path filter.

## Input

```json
{
  "path": "/Game/Blueprints",
  "property_name": "Sound",
  "property_type": "FSlateSound",
  "value_contains": "Explosion",
  "only_modified": true,
  "max_results": 50
}
```

| Parameter        | Required | Description                                                                                      |
| ---------------- | -------- | ------------------------------------------------------------------------------------------------ |
| `path`           | no       | Content path prefix to search (default: `/Game`). E.g. `/Game/CommonUI` to limit scope           |
| `property_name`  | no*      | Substring filter on property name (case-insensitive). E.g. `Sound` matches `HoveredSlateSound`   |
| `property_type`  | no*      | Substring filter on C++ property type (case-insensitive). E.g. `FSlateSound`, `FSoftObjectPath`  |
| `value_contains` | no*      | Substring filter on exported property value (case-insensitive). E.g. `Explosion`                 |
| `only_modified`  | no       | Only return properties that differ from parent class default (default: `true`)                    |
| `max_results`    | no       | Maximum number of results to return (default: `50`)                                              |

\* At least one of `property_name`, `property_type`, or `value_contains` is required.

## Output

```json
{
  "match_count": 2,
  "blueprints_scanned": 15,
  "path_filter": "/Game/Blueprints",
  "property_name_filter": "Sound",
  "only_modified": true,
  "results": [
    {
      "blueprint_name": "BP_ExplosionActor",
      "blueprint_path": "/Game/Blueprints/BP_ExplosionActor.BP_ExplosionActor",
      "property_name": "ExplosionSound",
      "property_type": "FSlateSound",
      "value": "/Game/Audio/SFX_Explosion.SFX_Explosion",
      "is_modified": true,
      "category": "Audio",
      "defined_in": "BP_ExplosionActor"
    }
  ]
}
```

## Result Entry Fields

| Field            | Type   | Description                                                       |
| ---------------- | ------ | ----------------------------------------------------------------- |
| `blueprint_name` | string | Name of the Blueprint containing the match                        |
| `blueprint_path` | string | Full asset path of the matching Blueprint                         |
| `property_name`  | string | Name of the matching property                                     |
| `property_type`  | string | C++ type of the property                                          |
| `value`          | string | Exported property value in UE text format                         |
| `is_modified`    | bool   | Whether the value differs from the parent class default           |
| `category`       | string | Property category (if available)                                  |
| `defined_in`     | string | Class that defines the property (if available)                    |

## Top-level Response Fields

| Field                   | Type   | Description                                                     |
| ----------------------- | ------ | --------------------------------------------------------------- |
| `match_count`           | int    | Number of matching results returned                             |
| `blueprints_scanned`    | int    | Number of Blueprints loaded and scanned                         |
| `results`               | array  | Array of matching property entries                              |
| `truncated`             | bool   | Present and `true` if results were capped by `max_results`      |
| `max_results`           | int    | The limit that was applied (only present when `truncated`)      |
| `path_filter`           | string | The path prefix that was used (if any)                          |
| `property_name_filter`  | string | The property name filter that was applied (if any)              |
| `property_type_filter`  | string | The property type filter that was applied (if any)              |
| `value_contains_filter` | string | The value substring filter that was applied (if any)            |
| `only_modified`         | bool   | Whether only modified properties were returned                  |

## Examples

### Find all Blueprints that reference an explosion sound

```json
{
  "value_contains": "Explosion",
  "property_type": "Sound"
}
```

### Find all modified Sound-related properties in CommonUI Blueprints

```json
{
  "path": "/Game/CommonUI",
  "property_name": "Sound",
  "only_modified": true
}
```

### Find Blueprints using a specific soft object path

```json
{
  "property_type": "FSoftObjectPath",
  "value_contains": "MyTexture",
  "max_results": 10
}
```

## Errors

| Condition                         | `isError` | Message                                                               |
| --------------------------------- | --------- | --------------------------------------------------------------------- |
| No search criteria provided       | `true`    | `Provide at least one of 'property_name', 'property_type', or 'value_contains'.` |

## Notes

- This tool loads each matching Blueprint into memory to inspect its CDO (Class Default Object). Use the `path` filter to limit scope and avoid loading the entire project.
- The `only_modified` flag defaults to `true`, which skips properties that match the parent class defaults. Set to `false` to see all matching properties regardless of modification state.
- Results are capped at `max_results` (default 50). If truncated, the response includes `"truncated": true`.
