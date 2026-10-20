# Tile Field

A full-screen instrument you play by touch. The screen is filled with thousands of small black tiles. Touch one and it flips to hot pink and plays a note. Pitch rises gradually across the grid and every note comes from one musical scale (A minor pentatonic), so anything you play together sounds harmonious.

Built with plain HTML, CSS and JavaScript. No frameworks, no dependencies, no build step, no microphone.

## Run it locally

Audio needs a user click first, and some features (recording) work best when the page is served from `localhost` rather than opened as a file.

**Option 1: VS Code (easiest)**
1. Open this folder in VS Code (`File > Open Folder`).
2. Install the recommended **Live Server** extension (VS Code will suggest it).
3. Right-click `index.html` and choose **Open with Live Server**.

**Option 2: Python**
```bash
python3 -m http.server 8000
```
Then open http://localhost:8000

**Option 3: Node**
```bash
npx serve .
```

You can also double-click `index.html`. It should work, but use a local server if recording misbehaves.

## How to play

| Action | Result |
| --- | --- |
| **Tap** a tile | It flips pink and its note keeps sounding until you tap it again. Tap several tiles to build a chord. |
| **Slide** across the grid | Tiles flip and play in a run like piano keys. Each note fades and its tile flips back to black. |
| **Multi-touch** | Use several fingers or both hands at once. Each finger plays independently. |
| **Recorder** (bottom left) | Press it, wait for the 3-2-1, play, then press it again. Your take becomes a loop. |
| **Loop bars** | Each loop appears as a slim pink bar next to the recorder. Double-click (or double-tap) a bar to remove that loop. |
| **Pink circle** (bottom centre) | Resets everything: tiles, sounds and loops. |

While recording, existing loops pause so you only hear and record the new take. When you stop, all loops restart together.

## How it works

- **Rendering:** a single `<canvas>`. Tiles are drawn at whole-pixel sizes so every gap is identical, and a flip is drawn as a vertical rotation with a little perspective. The grid refits when the window resizes and keeps your pink tiles.
- **Sound:** Web Audio API. One voice per pitch, each with a sustained output (tapped tiles) and a plucked output (slid tiles), plus a soft reverb and a compressor.
- **Recording:** the synth's final output is captured as raw audio (AudioWorklet, with a ScriptProcessor fallback). Loops play on a separate bus, so they are never re-recorded.
- **Input:** Pointer Events, with independent state per pointer for true multi-touch.

## Settings

Near the top of the script in `index.html`:

- `FLIP_MS`: flip duration
- `LIFE_MS`: how long a slid tile stays pink
- `SCALE`, `BASE_FREQ`, `COL_STEPS`, `ROW_STEPS`: pitch map
- `SIDE_FRAC`, `BOTTOM_FRAC`: frame thickness

## Browser support

Current Chrome, Edge, Firefox and Safari. Touch and stylus work through Pointer Events. Needs a click or tap before sound starts (a browser rule).

## Project structure

```
tile-field/
  index.html     the whole app (HTML, CSS and JavaScript)
  README.md
  .gitignore
  .vscode/       recommended extension for VS Code
```
