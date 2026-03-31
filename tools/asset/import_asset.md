# import_asset

Import an external file (texture, mesh, audio, etc.) into the Unreal project as an asset. Uses Unreal's automated asset import pipeline (`UAssetImportTask`), which auto-detects the appropriate factory based on the file extension.

## Input

```json
{
  "file_path": "C:/Art/hero_diffuse.png",
  "destination_path": "/Game/Textures",
  "asset_name": "T_Hero_Diffuse",
  "replace_existing": false,
  "save": true
}
```

| Parameter          | Required | Description                                                                                        |
| ------------------ | -------- | -------------------------------------------------------------------------------------------------- |
| `file_path`        | yes      | Path to the source file on disk. Absolute paths are used as-is; relative paths resolve against the project directory. |
| `destination_path` | yes      | Content path for the imported asset (e.g. `/Game/Textures`). Must start with `/Game` or `/Engine`. |
| `asset_name`       | no       | Name for the imported asset. Defaults to the source filename without extension.                     |
| `replace_existing` | no       | If `true`, overwrite any existing asset at the destination. Default: `false`.                       |
| `save`             | no       | If `true`, save the imported asset to disk. Default: `true`.                                       |

## Output

### Success

```json
{
  "source_file": "C:/Art/hero_diffuse.png",
  "destination_path": "/Game/Textures",
  "imported_assets": [
    {
      "asset_name": "T_Hero_Diffuse",
      "asset_path": "/Game/Textures/T_Hero_Diffuse",
      "asset_class": "Texture2D",
      "full_path": "/Game/Textures/T_Hero_Diffuse.T_Hero_Diffuse"
    }
  ],
  "imported_count": 1,
  "replaced_existing": false,
  "success": true
}
```

| Field               | Description                                                      |
| ------------------- | ---------------------------------------------------------------- |
| `source_file`       | The file path that was imported (as provided)                    |
| `destination_path`  | The content path where the asset was placed                      |
| `imported_assets`   | Array of imported asset details                                  |
| `imported_count`    | Number of assets created (usually 1, but some formats produce multiple) |
| `replaced_existing` | Whether the replace flag was set                                 |
| `success`           | `true` on success                                                |

### Imported Asset Object

| Field         | Description                                      |
| ------------- | ------------------------------------------------ |
| `asset_name`  | Name of the imported asset                       |
| `asset_path`  | Package path of the imported asset               |
| `asset_class` | UE class name (e.g. `Texture2D`, `StaticMesh`)  |
| `full_path`   | Full object path                                 |

### Error Cases

| Condition                     | Error message contains       |
| ----------------------------- | ---------------------------- |
| Missing `file_path`           | `"file_path"`                |
| Missing `destination_path`    | `"destination_path"`         |
| Source file not found          | `"not found"`                |
| Invalid destination path       | `"Invalid destination path"` |
| Asset exists, replace=false    | `"already exists"`           |
| Unsupported file type / bad file | `"Import failed"`         |

## Supported File Types

Unreal's import pipeline supports many formats. Common examples:

| Category  | Extensions                                 |
| --------- | ------------------------------------------ |
| Textures  | `.png`, `.jpg`, `.tga`, `.bmp`, `.exr`, `.hdr` |
| Meshes    | `.fbx`, `.obj`, `.gltf`, `.glb`           |
| Audio     | `.wav`, `.ogg`                             |
| Data      | `.csv` (DataTable), `.json`                |

The engine auto-detects the appropriate factory. No manual factory selection is needed.

## Notes

- Relative `file_path` values are resolved against `FPaths::ProjectDir()`, making it easy to import files stored alongside the project.
- The import runs in automated mode (`bAutomated = true`), suppressing all import dialogs.
- When `replace_existing` is `false` and the target asset already exists, the tool returns an error without modifying anything.
- Some file formats (e.g. FBX) may produce multiple assets (mesh + materials + textures). All are returned in `imported_assets`.
- The asset is rooted during import to prevent garbage collection, then unrooted after completion.
