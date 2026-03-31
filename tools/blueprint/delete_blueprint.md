# delete_blueprint

Delete a Blueprint asset. Checks for referencers before deleting unless `force` is set.

## Input

```json
{
  "asset_path": "/Game/BP_OldActor",
  "force": false
}
```

| Parameter    | Required | Description                                                 |
| ------------ | -------- | ----------------------------------------------------------- |
| `asset_path` | yes      | Asset path to delete                                        |
| `force`      | no       | Skip reference check and force delete (default: `false`)    |

## Output

```json
{
  "deleted": "/Game/BP_OldActor",
  "name": "BP_OldActor",
  "success": true
}
```

## Notes

- If the asset has referencers and `force` is `false`, returns an error listing the referencing assets.
- Force-deleting a referenced Blueprint will leave broken references in the referencing assets.
