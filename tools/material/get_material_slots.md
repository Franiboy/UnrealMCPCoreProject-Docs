# get_material_slots

Read material slot assignments from an actor's mesh components in the current level. Returns all material slots with their index, slot name, assigned material path, and component information. Optionally target a specific component by name.

## Input

```json
{
  "actor_name": "Wall_01",
  "component_name": "StaticMeshComponent0"
}
```

| Parameter        | Required | Description                                                              |
| ---------------- | -------- | ------------------------------------------------------------------------ |
| `actor_name`     | **yes**  | Actor name or label in the current level                                 |
| `component_name` | no       | Optional component name. If omitted, returns slots from all mesh components |

## Output

```json
{
  "actor_name": "StaticMeshActor_0",
  "actor_label": "Wall_01",
  "components": [
    {
      "component_name": "StaticMeshComponent0",
      "component_class": "StaticMeshComponent",
      "num_slots": 2,
      "slots": [
        {
          "index": 0,
          "slot_name": "Base",
          "material": "/Game/Materials/M_Brick.M_Brick",
          "material_name": "M_Brick",
          "material_class": "Material"
        },
        {
          "index": 1,
          "slot_name": "Trim",
          "material": "/Game/Materials/MI_WoodTrim.MI_WoodTrim",
          "material_name": "MI_WoodTrim",
          "material_class": "MaterialInstanceConstant"
        }
      ]
    }
  ],
  "total_components": 1,
  "total_slots": 2
}
```

| Field              | Type   | Description                                                        |
| ------------------ | ------ | ------------------------------------------------------------------ |
| `actor_name`       | string | Internal actor name                                                |
| `actor_label`      | string | Editor display label                                               |
| `components`       | array  | Array of mesh component objects                                    |
| `component_name`   | string | Component name                                                     |
| `component_class`  | string | Component class (e.g. `StaticMeshComponent`)                       |
| `num_slots`        | number | Number of material slots on this component                         |
| `slots`            | array  | Array of slot objects                                              |
| `index`            | number | Slot index                                                         |
| `slot_name`        | string | Slot name (from mesh) or `Slot_N` fallback                        |
| `material`         | string | Full path to assigned material, or `None`                          |
| `material_name`    | string | Material asset name                                                |
| `material_class`   | string | Material class (e.g. `Material`, `MaterialInstanceConstant`)       |
| `total_components` | number | Number of mesh components found                                    |
| `total_slots`      | number | Total slot count across all components                             |

## Examples

### Get all material slots on an actor

```json
{
  "actor_name": "Wall_01"
}
```

### Get slots from a specific component

```json
{
  "actor_name": "MyCharacter",
  "component_name": "BodyMesh"
}
```

## Notes

- The actor is found by matching against both its internal name (e.g. `StaticMeshActor_0`) and its editor label (e.g. `Wall_01`), case-insensitive
- When `component_name` is omitted, all `UMeshComponent` subclasses on the actor are included (`StaticMeshComponent`, `SkeletalMeshComponent`, etc.)
- Slot names come from the mesh asset's material slot list; if unavailable, the fallback `Slot_N` is used
- The `material` field is `None` when no material is assigned to a slot

## Errors

| Condition                          | `isError` | Message                                              |
| ---------------------------------- | --------- | ---------------------------------------------------- |
| Missing `actor_name`               | `true`    | `Missing required parameter 'actor_name'`            |
| Actor not found                    | `true`    | `Actor '...' not found in the current level`         |
| No mesh components                 | `true`    | `Actor '...' has no mesh components`                 |
| Named component not found          | `true`    | `Mesh component '...' not found on actor '...'`      |
| No editor world                    | `true`    | `No editor world available`                          |
