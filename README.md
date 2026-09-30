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
├── fx_outliner/    the tool (UI, XML config, icons, hotkey snippet)
├── fx_prefsaver/   UI-state persistence (window geometry via Maya optionVar)
├── fx_utils/       utils.py, qt_font_creator.py, proggy_tiny_sz.ttf
└── qt/             PySide / PySide2 compatibility shim
```

## Requirements

- Autodesk Maya with **Python 3 and PySide2** (Maya 2022–2024).
  Maya 2025+ ships PySide6, which is not supported yet (the tool uses
  `QRegExp`, removed in Qt 6).
- Windows paths: the tool builds its resource paths with `\\` separators.

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

## License

MIT. See [LICENSE](LICENSE).
