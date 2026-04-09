# fill_named_slot

Move a widget into a named slot of a UserWidget instance. The host widget must implement named slots (e.g. a child UserWidget with NamedSlot widgets). The content widget is removed from its current parent and placed into the slot.

## Input

```json
{
  "asset_path": "/Game/UI/WBP_SettingsPage",
  "widget_name": "SettingsPageBase_0",
  "slot_name": "SettingsContent",
  "content_widget_name": "Row_QualityPreset"
}
```

| Parameter             | Required | Description                                                                                    |
| --------------------- | -------- | ---------------------------------------------------------------------------------------------- |
| `asset_path`          | **yes**  | Asset path of the Widget Blueprint                                                             |
| `widget_name`         | **yes**  | Name of the UserWidget instance that owns the named slot (e.g. `SettingsPageBase_0`)           |
| `slot_name`           | **yes**  | Name of the named slot to fill (e.g. `SettingsContent`)                                        |
| `content_widget_name` | **yes**  | Name of an existing widget in the tree to move into the slot (e.g. `Row_QualityPreset`)        |

## Output

```json
{
  "widget_name": "SettingsPageBase_0",
  "slot_name": "SettingsContent",
  "content_widget_name": "Row_QualityPreset",
  "old_parent": "RootCanvas",
  "asset_path": "/Game/UI/WBP_SettingsPage"
}
```

| Field                 | Type   | Description                                                 |
| --------------------- | ------ | ----------------------------------------------------------- |
| `widget_name`         | string | Name of the host UserWidget that owns the slot              |
| `slot_name`           | string | Name of the slot that was filled                            |
| `content_widget_name` | string | Name of the widget that was moved into the slot             |
| `old_parent`          | string | Name of the widget's previous parent (or `(none)`)         |
| `asset_path`          | string | Full content path to the Widget Blueprint                   |

## Examples

### Fill a settings content slot

```json
{
  "asset_path": "/Game/UI/WBP_SettingsPage",
  "widget_name": "SettingsPageBase_0",
  "slot_name": "SettingsContent",
  "content_widget_name": "Row_QualityPreset"
}
```

### Place a header into a named slot

```json
{
  "asset_path": "/Game/UI/WBP_Dialog",
  "widget_name": "DialogTemplate_0",
  "slot_name": "HeaderSlot",
  "content_widget_name": "TitleText"
}
```

## Errors

| Condition                                  | `isError` | Message                                                                         |
| ------------------------------------------ | --------- | ------------------------------------------------------------------------------- |
| Missing required parameter                 | `true`    | `Missing required parameter '<param>'`                                          |
| Widget Blueprint not found                 | `true`    | `Widget Blueprint not found at '<asset_path>'`                                  |
| Host widget not found                      | `true`    | `Widget '<widget_name>' not found in '<asset_path>'`                            |
| Host widget does not support named slots   | `true`    | `Widget '<widget_name>' (class <class>) does not support named slots.`          |
| Named slot does not exist                  | `true`    | `Named slot '<slot_name>' not found on '<widget_name>'. Available slots: <...>` |
| Content widget not found                   | `true`    | `Content widget '<content_widget_name>' not found in '<asset_path>'`            |
| Content is ancestor of host                | `true`    | `Cannot fill slot: '<content_widget_name>' is an ancestor of (or is) the host widget '<widget_name>'.` |

## Notes

- The host widget must implement `INamedSlotInterface` (e.g. a `UUserWidget` child that contains `NamedSlot` widgets in its own Blueprint).
- The content widget is automatically removed from its current parent before being placed into the slot.
- The error message for a non-existent slot includes the list of available slot names, making it easy to discover valid values.
- The `component_name` parameter is accepted as an alias for `widget_name`.
