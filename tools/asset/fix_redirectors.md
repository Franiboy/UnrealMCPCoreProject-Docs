# fix_redirectors

Fix up and delete ObjectRedirectors in a content path. Redirectors are left behind when assets are renamed or moved. This tool loads each redirector, replaces all references to it with the actual destination asset, and deletes the redirector. Broken redirectors (with no valid destination) are deleted as well.

## Input

```json
{
  "path": "/Game/Blueprints"
}
```

| Parameter | Required | Description                                                                                                           |
| --------- | -------- | --------------------------------------------------------------------------------------------------------------------- |
| `path`    | no       | Content path to scan for redirectors (e.g. `/Game/Blueprints`). Scans recursively. Default: `/Game` (entire project). |

## Output

### Success (redirectors found and fixed)

```json
{
  "path": "/Game",
  "redirectors_found": 3,
  "redirectors_fixed": 2,
  "redirectors_broken": 1,
  "redirectors_failed": 0,
  "details": [
    {
      "redirector_path": "/Game/OldName.OldName",
      "destination": "/Game/NewName.NewName",
      "destination_name": "NewName",
      "status": "fixed"
    },
    {
      "redirector_path": "/Game/AnotherOld.AnotherOld",
      "destination": "/Game/Sub/AnotherNew.AnotherNew",
      "destination_name": "AnotherNew",
      "status": "fixed"
    },
    {
      "redirector_path": "/Game/BrokenRedir.BrokenRedir",
      "status": "broken_deleted"
    }
  ],
  "success": true
}
```

### Success (no redirectors found)

```json
{
  "path": "/Game/CleanFolder",
  "redirectors_found": 0,
  "redirectors_fixed": 0,
  "redirectors_broken": 0,
  "redirectors_failed": 0,
  "details": [],
  "success": true
}
```

| Field                 | Description                                                         |
| --------------------- | ------------------------------------------------------------------- |
| `path`                | The content path that was scanned                                   |
| `redirectors_found`   | Total number of ObjectRedirector assets discovered                  |
| `redirectors_fixed`   | Number of valid redirectors successfully fixed and deleted           |
| `redirectors_broken`  | Number of broken redirectors (null destination) that were deleted    |
| `redirectors_failed`  | Number of redirectors that could not be deleted                     |
| `details`             | Per-redirector detail array                                         |
| `success`             | Always `true` (the tool itself always succeeds; individual entries may fail) |

### Detail Object Fields

| Field              | Description                                                           |
| ------------------ | --------------------------------------------------------------------- |
| `redirector_path`  | Full path of the redirector asset                                     |
| `destination`      | Full path of the destination object (if valid)                        |
| `destination_name` | Name of the destination object (if valid)                             |
| `status`           | One of: `fixed`, `broken_deleted`, `failed`                           |
| `error`            | Error message (present only when `status` is `failed`)                |

### Status Values

| Status           | Meaning                                                            |
| ---------------- | ------------------------------------------------------------------ |
| `fixed`          | Redirector had a valid destination; references were replaced and redirector was deleted |
| `broken_deleted` | Redirector had no valid destination (broken); it was deleted       |
| `failed`         | Redirector could not be loaded or deleted                          |

## Notes

- Uses the Asset Registry to find `ObjectRedirector` assets in the specified path (recursive scan).
- For each redirector with a valid `DestinationObject`, `ObjectTools::ForceDeleteObjects` replaces all in-memory references with the destination before deleting the redirector.
- Broken redirectors (where `DestinationObject` is `null`) are simply deleted.
- Each redirector is processed individually, so a failure on one does not affect others.
- Calling this tool with no arguments scans the entire project (`/Game`).
- This tool is typically used after batch rename or move operations to clean up leftover redirectors.
- Complementary to `rename_asset` and `move_asset`, which create redirectors automatically.
