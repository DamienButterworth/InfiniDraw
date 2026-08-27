# InfiniDraw

An infinite-zoom drawing canvas that lives in a single HTML file. Draw anything, then zoom into any corner of your drawing forever — each level of zoom becomes fresh, full-resolution canvas to keep drawing on. There is no bottom and no edge.

No build step, no dependencies, no server. Just open `index.html`.

## Try it

Open `index.html` in any modern browser (desktop or mobile), or host the file anywhere static. That's the whole app.

## The idea

Most canvases are flat and finite. This one is recursive: zooming in never runs out of resolution because the scene is stored as a tree of nested frames. Draw a face, zoom into an eye, and you have a blank world the size of that eye to draw a whole new scene — and so on, as deep as you like. Zoom back out and it all nests together seamlessly.

Your work autosaves as you go, so closing and reopening the tab resumes exactly where you left off.

## Tools

| Tool | Shortcut | What it does |
|------|----------|--------------|
| Pen | `P` | Smooth freehand ink |
| Brush | `B` | Pressure-sensitive stroke (stylus) |
| Highlighter | `M` | Translucent marker |
| Line / Rectangle / Ellipse | `L` / `R` / `O` | Shape tools |
| Image | `I` | Drop a picture in and drag to place it |
| Select | `V` | Select, move, resize; `Delete`/`Backspace` removes |
| Eraser | `E` | Erase strokes |
| Pan | `H` · Space · two-finger drag | Move around the canvas |
| Magnifier | `G` | Loupe to inspect fine detail without zooming |

Plus: an ink color palette with a custom-color picker, a canvas background color, and undo/redo.

## Navigation

- **Zoom** — mouse wheel / trackpad pinch, two-finger pinch on touch, or the `+` / `−` buttons (keys `+` / `−`).
- **Fly to edges** — jump straight to the outermost or innermost drawing in the scene.
- **Pan** — hand tool, hold Space, middle/right-drag, or two-finger drag.

## Keyboard shortcuts

- Tools: `P` `B` `M` `L` `R` `O` `V` `E` `H` `G` `I`
- Zoom: `+` / `−`
- Undo / Redo: `Ctrl/Cmd+Z` / `Ctrl/Cmd+Shift+Z` (also `Ctrl/Cmd+Y`)
- Export: `Ctrl/Cmd+S`
- Fullscreen: `F`
- Show / hide toolbar: `Tab`

## Toolbar

The toolbar can be dragged to any edge (top, bottom, left, right) using the grip, and hidden entirely with `Tab` or the collapse button. Its position is remembered between sessions.

## Saving & files

- **Autosave** — the entire scene (drawing, camera position, and background) is continuously saved to your browser's `localStorage`, so reopening the tab restores your work. Nothing leaves your device.
- **Export** (`Ctrl/Cmd+S`) — downloads the drawing as JSON, gzip-compressed where the browser supports it.
- **Import** — load a previously exported `.json` (or `.json.gz`) file back in.

## Mobile

Built to work well on phones and tablets: pressure-sensitive drawing with a stylus, pinch-to-zoom, two-finger pan, fullscreen, and a movable toolbar. Returning to a backgrounded tab repaints automatically.

## Tech

A single self-contained `index.html`: vanilla JavaScript rendering to a `<canvas>`, no frameworks or dependencies. The scene is a tree of frames; the renderer walks the tree and draws each frame at its current on-screen scale, creating new nested frames as you zoom past a threshold.
