# rename_gameplay_tag

**Category:** Gameplay Ability System (GAS)  
**Source:** `Private/GAS/MCPGASTools_GameplayTags.cpp`

## Description

Renames a Gameplay Tag in the project INI file. A redirector from the old tag name to the new tag name is created automatically, so existing content references are preserved. Only INI-defined tags can be renamed; native C++ tags cannot.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `old_tag` | string | **Yes** | | The existing tag to rename (e.g. `Ability.Skill.Fireball`) |
| `new_tag` | string | **Yes** | | The new tag name (e.g. `Ability.Spell.Fireball`) |

## Response

```json
{
  "old_tag": "Ability.Skill.Fireball",
  "new_tag": "Ability.Spell.Fireball",
  "renamed": true
}
```

## Examples

### Rename a tag
```json
{
  "tool": "rename_gameplay_tag",
  "arguments": {
    "old_tag": "Ability.Skill.Fireball",
    "new_tag": "Ability.Spell.Fireball"
  }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing parameters | `"Missing required parameters 'old_tag' and 'new_tag'"` |
| Empty parameters | `"Parameters 'old_tag' and 'new_tag' must not be empty"` |
| Old tag not found | `"Tag '<tag>' not found"` |
| Native or protected tag | `"Failed to rename tag '<old>' to '<new>'. It may be defined in native code."` |
| GameplayTagsEditor unavailable | `"GameplayTagsEditor module is not available"` |
