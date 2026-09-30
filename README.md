# fx_outliner

Standalone extraction of the **FX Outliner** tool for Autodesk Maya, taken from
[cescocm/fxpt](https://github.com/cescocm/fxpt) (itself based on
[theetcher/fxpt](https://github.com/theetcher/fxpt) by Eugene Davydenko).

Extracted from upstream commit `d40c571`, ported to Python 3 and trimmed down
to a single self-contained module with no dependencies beyond Maya
(`maya.cmds`, PySide2/PySide6).

## Contents

```
fx_outliner/
├── fx_outliner.py               the tool
├── fx_outliner.json             extra outliner views
├── fx_outliner_user_menu.json   extra menu commands (MEL)
├── proggy_tiny_sz.ttf           monospace font for the search results table
├── icons/                       button icons
└── setup_hotkey.txt             MEL snippet binding the tool to F10
```

Settings (search options, current view, results window position and size)
are saved as JSON in the `fx_outliner` Maya optionVar.

## Requirements

- Autodesk Maya 2022 or later (Python 3):
  - Maya 2022–2024 use PySide2.
  - Maya 2025+ use PySide6.
- Windows, macOS or Linux. The menu items that open the JSON config files use
  Notepad on Windows, the default text editor on macOS (`open -t`) and
  `xdg-open` on Linux.

The regex search mode uses `QRegularExpression` (PCRE), replacing Qt 5's
`QRegExp`, so a few advanced patterns may behave differently. The wildcard mode
(`*`, `?`, `[...]`) still matches anywhere in the name, as before.

## Installation

1. Put the folder that *contains* `fx_outliner/` (this repository's root) on Maya's
   `PYTHONPATH`, e.g. in `Maya.env`:
   ```
   PYTHONPATH=C:/path/to/fx_outliner
   ```
2. In Maya's Python script editor:
   ```python
   from fx_outliner import fx_outliner
   fx_outliner.run()
   ```

To bind the tool to F10, run the MEL snippet in
`fx_outliner/setup_hotkey.txt` in Maya's script editor.

## Configuration

Two JSON files in `fx_outliner/` extend the tool. Both can be opened from
the tool's menu. Changes apply the next time FX Outliner is opened.

`fx_outliner.json` adds outliner views, listed after the built-in ones:

```json
{
    "views": [
        {
            "name": "Lights",
            "showDagOnly": true,
            "nodeTypes": ["ambientLight", "directionalLight", "spotLight"]
        }
    ]
}
```

Optional true/false keys (defaults in parentheses): `showShapes` (false),
`showShapesEnable` (true), `showDagOnly` (true), `showSetMembers` (true),
`showSetMembersEnable` (true), `expandObjects` (false),
`selectSetMembersEnable` (false). `nodeTypes` is the list of Maya node types
the view shows; omit it to show everything. Types that don't exist in the
running Maya (e.g. Arnold's `ai*` nodes when mtoa isn't loaded) are ignored, and
a view with none left is skipped with a warning, so reopen FX Outliner after
loading a plug-in. `selectCommand` (optional) makes selecting an item in the view
also select the objects it's assigned to when the "select set members" button
is on: `"materials"` for materials/textures, `"shadingGroups"` for shading
groups. Unknown keys produce a warning in the Script Editor.

The shipped config includes an **Arnold Materials** view listing the mtoa
surface and volume shaders.

`fx_outliner_user_menu.json` adds MEL commands to the tool's menu:

```json
{
    "commands": [
        {"name": "Outliner", "command": "OutlinerWindow;"}
    ]
}
```

## License

MIT. See [LICENSE](LICENSE).
