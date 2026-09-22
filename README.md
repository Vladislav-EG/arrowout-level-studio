# ArrowOut Level Studio

A browser-based level editor and generator for the **ArrowOut** puzzle: draw snakes on a grid, generate levels from any image, validate solvability, and export game-ready JSON.

**Live page:** https://vladislav-eg.github.io/arrowout-level-studio/

The app is a single static `index.html` — no build step, no server. The interface is available in Russian and English (the language is picked from the browser settings; use the **RU | EN** switch in the header to change it).

## What you can do

- **Draw** snakes cell by cell, set their direction and color, merge or split them.
- **Generate** a level from an image (drop a picture or use a sample): the picture is split into color zones and filled with snakes of the chosen length.
- **Validate** — the built-in solver checks whether the level can be cleared and reports the solve depth.
- **Save** levels into a personal library (left rail) and **export** them one by one as `level_NNN.json` or all at once as a `.zip`.
- **Import** game JSON files back into the editor.

## Loading real game levels

The public build ships with an empty "Game" tab. There are two ways to see real levels:

1. **Pack them into `levels.js`** (shows up in the *Game* tab, read-only):

   ```bash
   python3 tools/pack_levels.py path/to/levels > levels.js
   ```

   The script needs only Python 3, reads every `level_*.json` in the folder and writes a compact `window.PACKED_LEVELS = {...}` bundle. Replace the `levels.js` next to `index.html` with the generated one.

2. **Import into the library** (editable): click **Open JSON** in the header and pick several `.json` files or a `.zip` archive. Every `*.json` inside the archive (in any subfolder) is added to the library; the level number is taken from the file name (`level_024.json` → 24) or the next free number is used.

## Library storage warning

The library lives in your browser's `localStorage`. It is not synced anywhere and is lost when the site data is cleared or a different browser is used. **Export the library as a `.zip` regularly.**

## Level JSON format

```json
{
  "width": 6,
  "height": 5,
  "timer": { "seconds": 60 },
  "pieces": [
    {
      "id": 1,
      "dir": "down",
      "color": "#4285F4",
      "cells": [ { "x": 5, "y": 0 }, { "x": 5, "y": 1 }, { "x": 5, "y": 2 } ]
    }
  ]
}
```

- `cells` are listed from the head to the tail; neighbouring cells are 4-adjacent.
- `y` grows upwards (row 0 is the bottom row).
- `dir` is one of `up`, `down`, `left`, `right` and is the direction the snake moves when tapped.
- `timer.seconds` is the level time limit.

## Repository layout

| Path | Purpose |
| --- | --- |
| `index.html` | The whole application |
| `levels.js` | Packed game levels for the *Game* tab (empty placeholder by default) |
| `vendor/jszip.min.js` | JSZip 3.10.1, used for `.zip` import/export |
| `tools/pack_levels.py` | Packs a folder of level JSON files into `levels.js` |

## License

JSZip is distributed under the MIT / GPLv3 dual license (see the header of `vendor/jszip.min.js`).
