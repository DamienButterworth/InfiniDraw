# InfiniDraw

An infinite-zoom drawing canvas that lives in a single HTML file. Draw anything, then zoom into any corner of your drawing forever — each level of zoom becomes fresh, full-resolution canvas to keep drawing on. There is no bottom and no edge.

No build step, no dependencies, no server. Just open `index.html`.

## Try it

Open `index.html` in any modern browser (desktop or mobile), or host the file anywhere static. That's the whole app.

## The idea

Most canvases are flat and finite. This one is recursive: zooming in never runs out of resolution because the scene is stored as a tree of nested frames. Draw a face, zoom into an eye, and you have a blank world the size of that eye to draw a whole new scene — and so on, as deep as you like. Zoom back out and it all nests together seamlessly.

Your work autosaves as you go, so closing and reopening the tab resumes exactly where you left off.

## Modes

- **Creative** — free-form drawing: pen, brush, shapes, text, images.
- **Work** — flowchart / system-design mode: adds Node and Connect to the toolbar for boxes you draw links between.

Switch between them in Settings.

## Tools

| Tool | Shortcut | What it does |
|------|----------|--------------|
| Pen | `P` | Smooth freehand ink |
| Brush | `B` | Pressure-sensitive stroke (stylus) |
| Highlighter | `M` | Translucent marker |
| Line / Rectangle / Ellipse | `L` / `R` / `O` | Shape tools |
| Text | `T` | Click to place, type, click away to finish |
| Image | `I` | Drop a picture in and drag to place it |
| Select | `V` | Select, move, resize; `Delete`/`Backspace` removes |
| Region cut | — | Lift a piece of the drawing and move it; click again to change the cut shape |
| Eraser | `E` | Erase strokes |
| Pan | `H` · Space · two-finger drag | Move around the canvas |
| Magnifier | `G` | Hover to inspect fine detail without zooming |
| Eyedropper | `K` · Alt-click | Pick a colour straight off the canvas |
| Node / Connect | `N` / `C` | Flowchart boxes and the links between them — Work mode only |

Plus: an ink colour palette with a custom-color picker, adjustable brush size and opacity, a canvas background colour, and undo/redo.

## Navigation

- **Zoom** — mouse wheel / trackpad pinch, two-finger pinch on touch, or the `+` / `−` buttons (keys `+` / `−`).
- **Fly to edges** — jump straight to the outermost or innermost drawing in the scene.
- **Pan** — hand tool, hold Space, middle/right-drag, or two-finger drag.

## Keyboard shortcuts

- Tools: `P` `B` `M` `L` `R` `O` `T` `V` `E` `H` `G` `K` `I` (plus `N` `C` in Work mode)
- Layers panel: `Y`
- Zoom: `+` / `−`
- Undo / Redo: `Ctrl/Cmd+Z` / `Ctrl/Cmd+Shift+Z` (also `Ctrl/Cmd+Y`)
- Copy / Paste / Duplicate: `Ctrl/Cmd+C` / `Ctrl/Cmd+V` / `Ctrl/Cmd+D`
- Delete selection: `Delete` / `Backspace`
- Export: `Ctrl/Cmd+S`
- Fullscreen: `F`
- Show / hide toolbar: `Tab`

## Toolbar

- **Style** — Float (a movable pill) or Anchor (flush against the edge), both still hideable.
- **Position** — dock to any edge: top, bottom, left, or right.
- **Icon size** — 80%–180%, adjustable in Settings.
- **Reorder** — long-press an icon and drag it to a new spot (the rest wiggle to show reorder mode is active); a plain tap still selects the tool and a plain swipe still scrolls the strip. Dragging by the handle in Settings' tool list works the same way, and both stay in sync.
- **Show / hide icons** — drag an icon off the toolbar to hide it, or toggle it with the eye in Settings' tool list.
- **Hide the whole toolbar** — `Tab` or the collapse arrow.

Settings and Hide are always pinned — never reordered or hidden. Toolbar position, style, order, and visibility are all remembered between sessions.

## Canvas & drawing settings

Available in Settings:

- **Canvas colour** — background colour for the whole scene.
- **Grid** — cycles off → lines → dots; fades in/out smoothly as you zoom so it never looks cluttered.
- **Stroke stabiliser** — 0–90%, smooths out hand jitter while drawing.
- **Mirror** — off, vertical, horizontal, or 4-way symmetry; pen/brush/marker strokes are mirrored live as you draw.
- **Reset everything** — erases your drawing and puts every setting back to default.

## Layers

A single global layer stack (Photoshop/GIMP-style) shared across every zoom level — "Layer 1" means the same layer no matter how deep you've zoomed. Open the panel with `Y` or the Layers button to add, duplicate, reorder, merge down, or delete layers, and to toggle each layer's visibility, lock it, or adjust its opacity. Each layer also has its own "fly to outermost/innermost drawing" shortcut, scoped to just that layer's content.

## Saving & files

- **Autosave** — the entire scene (drawing, camera position, and background) is continuously saved to your browser's `localStorage`, so reopening the tab restores your work. Nothing leaves your device.
- **Export** (`Ctrl/Cmd+S`) — downloads the drawing as JSON, gzip-compressed where the browser supports it.
- **Import** — load a previously exported `.json` (or `.json.gz`) file back in.

## Custom tutorial (local only)

The first-run tutorial you see with no saved drawing is built into the app, but you can swap in
your own by baking a drawing you've exported straight into `index.html`:

```
.\set-tutorial.cmd path\to\your-export.json.gz
```

Run it with no arguments to clear the override and go back to the built-in tutorial:

```
.\set-tutorial.cmd
```

It accepts either `.json` or `.json.gz` — whatever Export gives you, no need to rename or
decompress it first. Under the hood it rewrites one line in `index.html` (`CUSTOM_TUTORIAL_DATA`,
`null` by default) with your drawing verbatim, so there's nothing else to configure and nothing
in the UI referencing it. Because that line lives in the tracked file, it only stays private as
long as you don't commit it while it's set — clear it (no arguments) before committing or
sharing this file. (`set-tutorial.ps1` does the work; `set-tutorial.cmd` is a thin wrapper so it
runs without touching PowerShell's script execution policy.)

## Mobile

Built to work well on phones and tablets: pressure-sensitive drawing with a stylus, pinch-to-zoom, two-finger pan, fullscreen, and a movable toolbar. Returning to a backgrounded tab repaints automatically.

## Tech

A single self-contained `index.html`: vanilla JavaScript rendering to a `<canvas>`, no frameworks or dependencies. The scene is a tree of frames; the renderer walks the tree and draws each frame at its current on-screen scale, creating new nested frames as you zoom past a threshold.
