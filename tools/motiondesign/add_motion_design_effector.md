# add_motion_design_effector

Link an Effector actor to a Cloner so it affects the cloner's instances.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `cloner_name` | string | Yes | Name or label of the Cloner actor |
| `effector_name` | string | Yes | Name or label of the Effector actor to link |

## Example

**Request:**
```json
{
  "tool": "add_motion_design_effector",
  "arguments": {
    "cloner_name": "MyCloner",
    "effector_name": "MyEffector"
  }
}
```

**Response:**
```json
{
  "cloner_name": "MyCloner",
  "effector_name": "MyEffector",
  "linked": true,
  "effector_count": 1
}
```

## Notes

- Requires the ClonerEffector plugin (UE 5.5 experimental)
- Automatically creates the Effector extension on the Cloner if one doesn't exist
- Returns error if the effector is already linked to the cloner
- Returns error if either actor is not found or not the correct type
