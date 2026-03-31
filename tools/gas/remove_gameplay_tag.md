# remove_gameplay_tag

**Category:** Gameplay Ability System (GAS)  
**Source:** `Private/GAS/MCPGASTools_GameplayTags.cpp`

## Description

Removes a Gameplay Tag from the project INI file. Only INI-defined tags can be removed; native C++ tags (defined with `UE_DEFINE_GAMEPLAY_TAG`) cannot be deleted. Tags that are referenced by content may also fail to delete.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `tag` | string | **Yes** | | The tag to remove (e.g. `Ability.Skill.Fireball`) |

## Response

```json
{
  "tag": "Ability.Skill.Fireball",
  "removed": true
}
```

## Examples

### Remove a tag
```json
{ "tool": "remove_gameplay_tag", "arguments": { "tag": "Ability.Skill.Fireball" } }
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `tag` parameter | `"Missing required parameter 'tag'"` |
| Empty `tag` parameter | `"Parameter 'tag' must not be empty"` |
| Tag not found | `"Tag '<tag>' not found"` |
| Tag node not found | `"Could not find tag node for '<tag>'"` |
| Referenced by content or native | `"Failed to remove tag '<tag>'. It may be referenced by content or defined in native code."` |
| GameplayTagsEditor unavailable | `"GameplayTagsEditor module is not available"` |
