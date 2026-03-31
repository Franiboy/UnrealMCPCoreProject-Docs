# get_collision_profiles

List collision presets and channels configured in the project. Returns all collision profiles with their object type, collision enabled state, and custom responses to each channel.

## Input

```json
{}
```

| Parameter      | Required | Default | Description                                                                    |
| -------------- | -------- | ------- | ------------------------------------------------------------------------------ |
| `profile_name` | no       | —       | If specified, return details for only this profile. Otherwise all are returned. |

## Output

### When listing all profiles

| Field      | Type   | Description                  |
| ---------- | ------ | ---------------------------- |
| `count`    | number | Number of profiles returned  |
| `profiles` | array  | Array of profile objects     |

### When querying a single profile

| Field     | Type   | Description         |
| --------- | ------ | ------------------- |
| `profile` | object | The profile details |

### Profile object

| Field                | Type    | Description                                              |
| -------------------- | ------- | -------------------------------------------------------- |
| `name`               | string  | Profile name (e.g. `BlockAll`, `Pawn`)                   |
| `collision_enabled`  | string  | Collision mode: `NoCollision`, `QueryOnly`, `PhysicsOnly`, `QueryAndPhysics`, `ProbeOnly`, `QueryAndProbe` |
| `object_type_name`   | string  | Object type channel name                                 |
| `object_type_channel`| number  | Numeric collision channel value                          |
| `can_modify`         | boolean | Whether the profile can be modified at runtime           |
| `custom_responses`   | array   | Custom collision responses (channel name + response)     |

### Response object

| Field      | Type   | Description                              |
| ---------- | ------ | ---------------------------------------- |
| `channel`  | string | Collision channel name                   |
| `response` | string | Response: `Ignore`, `Overlap`, `Block`   |

## Examples

### List all collision profiles

```json
{}
```

### Get a specific profile

```json
{
  "profile_name": "Pawn"
}
```

## Errors

| Condition                 | Error message                           |
| ------------------------- | --------------------------------------- |
| Profile name not found    | `Profile '<name>' not found`            |
| System not available      | `CollisionProfile system not available` |
