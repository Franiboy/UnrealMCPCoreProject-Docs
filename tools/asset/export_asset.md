# export_asset

Export an Unreal asset to an external file on disk. The file extension determines the export format, and the appropriate exporter is auto-detected from the asset type and file extension.

## Input

```json
{
  "asset_path": "/Game/Textures/T_Wood",
  "output_path": "C:/Export/wood.png",
  "overwrite": true
}
```

| Parameter     | Required | Description                                                                                           |
| ------------- | -------- | ----------------------------------------------------------------------------------------------------- |
| `asset_path`  | yes      | Content path of the asset to export (e.g. `/Game/Textures/T_Wood`). Short form accepted.              |
| `output_path` | yes      | File path to write to. Extension determines format. Relative paths resolve against the project directory. |
| `overwrite`   | no       | If `true` (default), overwrite existing files. If `false`, fail when the file already exists.          |

## Output

### Success

```json
{
  "asset_path": "/Game/Textures/T_Wood",
  "asset_class": "Texture2D",
  "asset_name": "T_Wood",
  "output_path": "C:/Export/wood.png",
  "output_path_absolute": "C:/Export/wood.png",
  "format": "png",
  "file_size_bytes": 32768,
  "exporter": "TextureExporterPNG",
  "success": true
}
```

| Field                  | Description                                                  |
| ---------------------- | ------------------------------------------------------------ |
| `asset_path`           | The content path that was exported (as provided)             |
| `asset_class`          | UE class name of the exported asset                          |
| `asset_name`           | Name of the exported asset                                   |
| `output_path`          | The output file path (as provided)                           |
| `output_path_absolute` | Fully resolved absolute output file path                     |
| `format`               | File extension used for export                               |
| `file_size_bytes`      | Size of the exported file in bytes                           |
| `exporter`             | UE exporter class used (e.g. `TextureExporterPNG`)           |
| `success`              | `true` on success                                            |

### Error Cases

| Condition                          | Error message contains        |
| ---------------------------------- | ----------------------------- |
| Missing `asset_path`               | `"asset_path"`                |
| Missing `output_path`              | `"output_path"`               |
| No file extension                  | `"file extension"`            |
| Asset not found                    | `"not found"`                 |
| No exporter for type+extension     | `"No exporter found"`         |
| File exists and overwrite=false    | `"already exists"`            |
| Cannot create output directory     | `"Failed to create"`          |

## Supported Export Formats

The engine auto-detects the appropriate exporter. Common combinations:

| Asset Type     | Supported Extensions             |
| -------------- | -------------------------------- |
| Texture2D      | `.png`, `.tga`, `.exr`, `.bmp`   |
| StaticMesh     | `.fbx`, `.obj`                   |
| SkeletalMesh   | `.fbx`                           |
| AnimSequence   | `.fbx`                           |
| SoundWave      | `.wav`                           |
| DataTable      | `.csv`, `.json`                  |

## Notes

- Relative `output_path` values are resolved against `FPaths::ProjectDir()`.
- The export runs in automated mode (`bAutomated = true`), suppressing all export dialogs.
- Output directories are created automatically if they don't exist.
- Uses `UAssetExportTask` + `UExporter::RunAssetExportTask()` for the export pipeline.
- The exporter is found via `UExporter::FindExporter()` using the asset object and file extension.
- This is the counterpart to `import_asset`.
