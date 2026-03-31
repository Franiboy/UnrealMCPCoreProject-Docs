# list_actors

List actors in the current editor level. Supports filtering by class name, actor tag, editor folder path, and name search. Returns actor name, label, class, transform, and tags for each match. Supports pagination via offset/limit.

## Input

```json
{
  "class": "StaticMeshActor",
  "tag": "MyTag",
  "folder": "/Lighting",
  "search": "Wall",
  "include_transform": true,
  "include_tags": true,
  "limit": 100,
  "offset": 0
}
```

| Parameter           | Required | Description                                                                      |
| ------------------- | -------- | -------------------------------------------------------------------------------- |
| `class`             | no       | Filter by actor class name (case-insensitive partial match).                     |
| `tag`               | no       | Filter by actor tag (case-insensitive exact match). Only tagged actors returned. |
| `folder`            | no       | Filter by editor folder path prefix (e.g. `/MyFolder`).                          |
| `search`            | no       | Search by actor name or label (case-insensitive substring).                      |
| `include_transform` | no       | Include full transform per actor (default: `true`).                              |
| `include_tags`      | no       | Include tags array per actor (default: `true`).                                  |
| `limit`             | no       | Maximum number of actors to return (default: `1000`).                            |
| `offset`            | no       | Number of matching actors to skip for pagination (default: `0`).                 |

All parameters are optional — calling with `{}` returns up to 1000 actors.

## Output

### Success

```json
{
  "total": 42,
  "offset": 0,
  "count": 3,
  "filter_class": "Light",
  "actors": [
    {
      "name": "PointLight_0",
      "label": "PointLight",
      "class": "PointLight",
      "folder": "/Lighting",
      "transform": {
        "location_x": 100.0,
        "location_y": 200.0,
        "location_z": 300.0,
        "rotation_pitch": 0.0,
        "rotation_yaw": 0.0,
        "rotation_roll": 0.0,
        "scale_x": 1.0,
        "scale_y": 1.0,
        "scale_z": 1.0
      },
      "tags": ["MyTag"]
    }
  ]
}
```

| Field           | Description                                                                  |
| --------------- | ---------------------------------------------------------------------------- |
| `total`         | Total number of actors matching all filters (before pagination)              |
| `offset`        | Offset applied                                                               |
| `count`         | Number of actors returned in this page                                       |
| `filter_class`  | Echo of the class filter (present only when filter is active)                |
| `filter_tag`    | Echo of the tag filter (present only when filter is active)                  |
| `filter_folder` | Echo of the folder filter (present only when filter is active)               |
| `filter_search` | Echo of the search filter (present only when filter is active)               |
| `actors`        | Array of actor objects                                                       |

Each actor object contains:

| Field       | Description                                                          |
| ----------- | -------------------------------------------------------------------- |
| `name`      | Internal UE actor name (e.g. `StaticMeshActor_0`)                   |
| `label`     | Display label in the editor                                          |
| `class`     | Actor class name                                                     |
| `folder`    | Editor folder path (present only if actor is in a folder)            |
| `transform` | Location/rotation/scale (present when `include_transform=true`)      |
| `tags`      | Array of actor tag strings (present when `include_tags=true` and actor has tags) |

### Error Cases

| Condition       | Error message contains        |
| --------------- | ----------------------------- |
| No editor world | `"No editor world available"` |

## Notes

- Uses `TActorIterator<AActor>` to iterate all actors in the persistent level.
- Class filter uses case-insensitive partial match — `"Light"` matches `PointLight`, `DirectionalLight`, `SpotLight`, etc.
- Tag filter uses case-insensitive exact match against the actor's `Tags` array.
- Folder filter matches the actor's editor folder path prefix (via `GetFolderPath()`).
- Name search matches against both the internal name (`GetName()`) and the editor label (`GetActorLabel()`).
- Filters are combined with AND logic — an actor must pass all active filters to be included.
- Active filters are echoed back in the response for clarity.
- Pagination is applied after filtering — `total` reflects the full filtered count.
