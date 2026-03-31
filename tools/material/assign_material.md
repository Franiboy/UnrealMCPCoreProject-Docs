# assign_material

Assign a material to a mesh component slot on an actor in the current level. Finds the actor by name or label, locates its mesh component, and sets the material at the specified slot index.

## Input

```json
{
  "actor_name": "Floor_01",
  "material": "/Game/Materials/M_Floor",
  "slot_index": 0,
  "component_name": "StaticMeshComponent0"
}
```

| Parameter        | Required | Description                                                            |
| ---------------- | -------- | ---------------------------------------------------------------------- |
| `actor_name`     | **yes**  | Actor name or label in the current level (case-insensitive)            |
| `material`       | **yes**  | Content path to the material or material instance to assign            |
| `slot_index`     | no       | Material slot index (default: 0)                                       |
| `component_name` | no       | Mesh component name. If omitted, uses the first mesh component found   |

## Output

```json
{
  "actor_name": "StaticMeshActor_0",
  "actor_label": "Floor_01",
  "component": "StaticMeshComponent0",
  "component_class": "StaticMeshComponent",
  "slot_index": 0,
  "total_slots": 1,
  "material": "/Game/Materials/M_Floor.M_Floor",
  "previous_material": "/Engine/BasicShapes/BasicShapeMaterial.BasicShapeMaterial"
}
```

| Field               | Type   | Description                                           |
| ------------------- | ------ | ----------------------------------------------------- |
| `actor_name`        | string | Internal actor name                                   |
| `actor_label`       | string | Editor display label                                  |
| `component`         | string | Mesh component name                                   |
| `component_class`   | string | Component class (e.g. `StaticMeshComponent`)          |
| `slot_index`        | number | Material slot that was assigned                       |
| `total_slots`       | number | Total material slots on the component                 |
| `material`          | string | Full path to the assigned material                    |
| `previous_material` | string | Full path to the material that was replaced, or `None`|

## Examples

### Assign material to an actor by label

```json
{
  "actor_name": "Floor_01",
  "material": "/Game/Materials/M_Marble"
}
```

### Assign to a specific slot

```json
{
  "actor_name": "Wall_Multi",
  "material": "/Game/Materials/MI_BrickRed",
  "slot_index": 2
}
```

### Assign to a named component

```json
{
  "actor_name": "MyCharacter",
  "material": "/Game/Materials/MI_Skin",
  "component_name": "BodyMesh"
}
```

## Notes

- The actor is found by matching against both its internal name (e.g. `StaticMeshActor_0`) and its editor label (e.g. `Floor_01`), case-insensitive
- When `component_name` is omitted, the first `UMeshComponent` on the actor is used (works for `StaticMeshComponent`, `SkeletalMeshComponent`, etc.)
- If the mesh component has no mesh assigned (0 material slots), only slot 0 is allowed — the material override is still stored
- The `previous_material` field shows what was assigned before, useful for undo logic

## Errors

| Condition                          | `isError` | Message                                              |
| ---------------------------------- | --------- | ---------------------------------------------------- |
| Missing `actor_name`               | `true`    | `Missing required parameter 'actor_name'`            |
| Missing `material`                 | `true`    | `Missing required parameter 'material'`              |
| Actor not found                    | `true`    | `Actor '...' not found in the current level`         |
| Material not found                 | `true`    | `Material not found: '...'`                          |
| No mesh component                  | `true`    | `Actor '...' has no mesh component`                  |
| Named component not found          | `true`    | `Mesh component '...' not found on actor '...'`      |
| Slot index out of range            | `true`    | `Slot index N out of range...`                       |
| No editor world                    | `true`    | `No editor world available`                          |
