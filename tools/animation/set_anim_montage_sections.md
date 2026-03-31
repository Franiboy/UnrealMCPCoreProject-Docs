# set_anim_montage_sections

Add, remove, or configure sections on an AnimMontage. Supports three actions: `add` (create a new section), `remove` (delete an existing section), and `set_next_section` (link a section to its successor).

## Input

```json
{
  "asset_path": "/Game/Animations/AM_Attack",
  "action": "add",
  "section_name": "Recovery",
  "start_time": 1.5,
  "next_section": "Default"
}
```

| Parameter      | Required | Default | Description                                                             |
| -------------- | -------- | ------- | ----------------------------------------------------------------------- |
| `asset_path`   | **yes**  | —       | Content path to the AnimMontage.                                        |
| `action`       | **yes**  | —       | `"add"`, `"remove"`, or `"set_next_section"`.                           |
| `section_name` | varies   | —       | Section name (required for all actions).                                |
| `start_time`   | varies   | —       | Start time in seconds (required for `add`).                             |
| `next_section` | no       | —       | Section to transition to (for `add` and `set_next_section`).            |

## Output

| Field            | Type   | Description                                    |
| ---------------- | ------ | ---------------------------------------------- |
| `asset_path`     | string | Full content path of the montage               |
| `action`         | string | Action that was performed                      |
| `sections`       | array  | Current sections: `[{name, start_time, next_section}, ...]` |
| `total_sections` | number | Total section count after operation            |

Additional fields per action:
- **add**: `section_name`, `start_time`, `index`
- **remove**: `removed_section`
- **set_next_section**: `section_name`, `old_next_section`, `new_next_section`

## Examples

### Add a section

```json
{
  "asset_path": "/Game/Animations/AM_Attack",
  "action": "add",
  "section_name": "Recovery",
  "start_time": 1.5
}
```

### Remove a section

```json
{
  "asset_path": "/Game/Animations/AM_Attack",
  "action": "remove",
  "section_name": "Recovery"
}
```

### Link sections

```json
{
  "asset_path": "/Game/Animations/AM_Attack",
  "action": "set_next_section",
  "section_name": "Default",
  "next_section": "Recovery"
}
```

## Errors

| Condition                 | `isError` | Message                                              |
| ------------------------- | --------- | ---------------------------------------------------- |
| Missing `asset_path`      | `true`    | `Missing required parameter 'asset_path'`            |
| Missing `action`          | `true`    | `Missing required parameter 'action'`                |
| Invalid action            | `true`    | `Unknown action '...'. Must be 'add', 'remove', ...` |
| Asset not found           | `true`    | `Animation asset not found: '...'`                   |
| Not an AnimMontage        | `true`    | `Asset '...' is not an AnimMontage`                  |
| Missing `section_name`    | `true`    | `Missing 'section_name' for ... action`              |
| Missing `start_time`      | `true`    | `Missing 'start_time' for add action`                |
| Section already exists    | `true`    | `Section '...' already exists`                       |
| Section not found         | `true`    | `Section '...' not found`                            |
| Cannot remove last        | `true`    | `Cannot remove the last remaining section`           |
