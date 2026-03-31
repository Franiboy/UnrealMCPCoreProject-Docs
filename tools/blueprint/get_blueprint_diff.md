# get_blueprint_diff

Compare two Blueprints structurally and return a detailed diff across variables, functions, components, interfaces, event dispatchers, graphs, metadata, and class defaults.

## Input

```json
{
  "asset_path_a": "/Game/Blueprints/BP_PlayerOld",
  "asset_path_b": "/Game/Blueprints/BP_PlayerNew",
  "include_graphs": true,
  "include_defaults": true
}
```

|| Parameter          | Required | Description                                                     |
|| ------------------ | -------- | --------------------------------------------------------------- |
|| `asset_path_a`     | yes      | Asset path of the first Blueprint (baseline)                    |
|| `asset_path_b`     | yes      | Asset path of the second Blueprint (comparison target)          |
|| `include_graphs`   | no       | Include graph-level diff (default: `true`)                      |
|| `include_defaults` | no       | Include class defaults diff (default: `true`)                   |

## Output

```json
{
  "blueprint_a": {
    "name": "BP_PlayerOld",
    "asset_path": "/Game/Blueprints/BP_PlayerOld"
  },
  "blueprint_b": {
    "name": "BP_PlayerNew",
    "asset_path": "/Game/Blueprints/BP_PlayerNew"
  },
  "metadata": {
    "has_differences": false,
    "differences": []
  },
  "variables": {
    "has_differences": true,
    "added": [
      { "name": "NewHealth", "type": "Float", "..." : "..." }
    ],
    "removed": [
      { "name": "OldHealth", "type": "Int", "..." : "..." }
    ],
    "modified": [
      {
        "name": "Speed",
        "differences": [
          { "field": "type", "a": "Float", "b": "Double" }
        ]
      }
    ]
  },
  "functions": {
    "has_differences": false,
    "added": [],
    "removed": [],
    "modified": []
  },
  "components": {
    "has_differences": false,
    "added": [],
    "removed": [],
    "modified": []
  },
  "interfaces": {
    "has_differences": false,
    "added": [],
    "removed": []
  },
  "event_dispatchers": {
    "has_differences": false,
    "added": [],
    "removed": [],
    "modified": []
  },
  "graphs": {
    "has_differences": false,
    "added": [],
    "removed": [],
    "modified": []
  },
  "class_defaults": {
    "has_differences": false,
    "added": [],
    "removed": [],
    "modified": []
  },
  "summary": {
    "identical": false,
    "total_differences": 3,
    "categories_with_changes": ["variables"]
  }
}
```

## Diff Categories

|| Category             | Checks                                                                     |
|| -------------------- | -------------------------------------------------------------------------- |
|| `metadata`           | Parent class, blueprint type, compile status, description, category        |
|| `variables`          | Added, removed, and modified variables (type, default, category, flags)    |
|| `functions`          | Added, removed, and modified functions (params, return type, flags)        |
|| `components`         | Added, removed, and modified components (class, transform, properties)     |
|| `interfaces`         | Added and removed interface implementations                                |
|| `event_dispatchers`  | Added, removed, and modified event dispatchers (params)                    |
|| `graphs`             | Added, removed, and modified graphs (node count, graph type) — optional    |
|| `class_defaults`     | Added, removed, and modified CDO property values — optional                |

## Summary Fields

|| Field                     | Type   | Description                                          |
|| ------------------------- | ------ | ---------------------------------------------------- |
|| `identical`               | bool   | `true` if no differences were found                  |
|| `total_differences`       | int    | Total count of all differences across all categories |
|| `categories_with_changes` | array  | Names of categories that have at least one change    |

## Notes

- Comparing a Blueprint against itself always returns `identical: true` with zero differences.
- Set `include_graphs: false` to skip graph comparison (faster for large Blueprints).
- Set `include_defaults: false` to skip class defaults comparison.
- Variables, functions, components, and event dispatchers are matched by name. Items present in A but not B appear as `removed`; items in B but not A appear as `added`.
- Modified items include a `differences` array listing each changed field with its `a` (baseline) and `b` (comparison) values.
