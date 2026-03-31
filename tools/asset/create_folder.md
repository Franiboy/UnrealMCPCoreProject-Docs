# create_folder

Create a content browser folder in the project. Creates intermediate directories as needed (like `mkdir -p`). Idempotent — succeeds even if the folder already exists.

## Input

```json
{
  "folder_path": "/Game/MyFolder/SubFolder"
}
```

| Parameter     | Required | Description                                                                 |
| ------------- | -------- | --------------------------------------------------------------------------- |
| `folder_path` | yes      | Content path for the folder (must start with `/Game` or `/Engine`).         |

## Output

### Success

```json
{
  "folder_path": "/Game/MyFolder/SubFolder",
  "disk_path": "C:/Projects/MyProject/Content/MyFolder/SubFolder/",
  "already_existed": false,
  "success": true
}
```

| Field             | Description                                                     |
| ----------------- | --------------------------------------------------------------- |
| `folder_path`     | The content path that was created                               |
| `disk_path`       | The resolved on-disk path                                       |
| `already_existed` | `true` if the folder already existed before the call            |
| `success`         | `true` if the operation succeeded                               |

### Already Exists

```json
{
  "folder_path": "/Game/MyFolder/SubFolder",
  "disk_path": "C:/Projects/MyProject/Content/MyFolder/SubFolder/",
  "already_existed": true,
  "success": true
}
```

The tool succeeds with `already_existed: true` — it never fails because a folder already exists.

## Errors

| Scenario                     | Error message contains                |
| ---------------------------- | ------------------------------------- |
| Invalid path prefix          | `"must start with /Game or /Engine"`  |
| Could not resolve disk path  | `"Could not resolve disk path"`       |
| Failed to create on disk     | `"Failed to create directory"`        |
| Missing `folder_path`        | `"folder_path"`                       |

## Notes

- Creates the full directory tree on disk using `IPlatformFile::CreateDirectoryTree()`.
- Registers the path with the Asset Registry via `AddPath()` so it appears immediately in the Content Browser.
- No undo — folder creation is considered a lightweight, non-destructive operation.
- Useful before `move_asset` or `duplicate_asset` to ensure the destination folder exists.
