# add_gameplay_tag

**Category:** Gameplay Ability System (GAS)  
**Source:** `Private/GAS/MCPGASTools_GameplayTags.cpp`

## Description

Adds a new Gameplay Tag to the project INI file. If the tag already exists, returns success with `already_exists: true`. Parent tags in the hierarchy are created implicitly (e.g. adding `Ability.Skill.Fireball` will create `Ability` and `Ability.Skill` as implicit parent tags).

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `tag` | string | **Yes** | | The tag to add, using dot-separated hierarchy (e.g. `Ability.Skill.Fireball`) |
| `comment` | string | No | | Optional comment/description for the tag |

## Response

```json
{
  "tag": "Ability.Skill.Fireball",
  "created": true,
  "comment": "Fire damage ability"
}
```

When the tag already exists:
```json
{
  "tag": "Ability.Skill.Fireball",
  "already_exists": true,
  "message": "Tag 'Ability.Skill.Fireball' already exists"
}
```

## Examples

### Add a simple tag
```json
{ "tool": "add_gameplay_tag", "arguments": { "tag": "Ability.Skill.Fireball" } }
```

### Add a tag with comment
```json
{
  "tool": "add_gameplay_tag",
  "arguments": {
    "tag": "Status.Buff.Haste",
    "comment": "Movement speed increase buff"
  }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `tag` parameter | `"Missing required parameter 'tag'"` |
| Empty `tag` parameter | `"Parameter 'tag' must not be empty"` |
| GameplayTagsEditor unavailable | `"GameplayTagsEditor module is not available"` |
| INI write failure | `"Failed to add tag '<tag>' to INI"` |
