# fx_outliner

Standalone extraction of the **FX Outliner** tool for Autodesk Maya, taken from
[cescocm/fxpt](https://github.com/cescocm/fxpt) (itself based on
[theetcher/fxpt](https://github.com/theetcher/fxpt) by Eugene Davydenko).

Only `fx_outliner` and the fxpt modules it depends on are included, copied
from upstream commit `d40c571` and then ported to Python 3. The `fxpt` package
layout is kept so the original imports still resolve.

## Contents

```
fxpt/
├── fx_outliner/    the tool (UI, JSON config, icons, hotkey snippet)
├── fx_prefsaver/   UI-state persistence (window geometry via Maya optionVar)
├── fx_utils/       utils.py, qt_font_creator.py, proggy_tiny_sz.ttf
└── qt/             PySide / PySide2 compatibility shim
```

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

1. Put the folder that *contains* `fxpt/` (this repository's root) on Maya's
   `PYTHONPATH`, e.g. in `Maya.env`:
   ```
   PYTHONPATH=C:/path/to/fx_outliner
   ```
2. In Maya's Python script editor:
   ```python
   from fxpt.fx_outliner import fx_outliner
   fx_outliner.run()
   ```

`fxpt/fx_outliner/setup_hotkey.txt` has a MEL snippet that binds the tool to
F10. That snippet uses `from fx_outliner import fx_outliner`; with the layout
above, change it to `from fxpt.fx_outliner import fx_outliner`.

## Configuration

Two JSON files in `fxpt/fx_outliner/` extend the tool. Both can be opened from
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
the view shows; omit it to show everything. Unknown keys produce a warning in
the Script Editor.

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
