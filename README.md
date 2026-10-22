# BitReel

**Turn video into clean, looping pixel art.**

BitReel is a single-page browser tool that converts video footage into pixel-art animation loops. It handles live action, renders and AI-generated "pixel art style" video, and exports GIFs, video, PNG frames, sprite sheets and a ready-to-import Unity package.

Everything runs locally in your browser. Your video never leaves your machine.

![BitReel interface: source video with crop and pixel grid on the left, the grid-snapped pixel-art loop on the right, tool tabs and the frame strip below](interface.jpg)

---

## Contents

- [Features](#features)
- [Quick start](#quick-start)
- [Typical workflow](#typical-workflow)
- [Interface overview](#interface-overview)
- [Source panel](#source-panel)
- [Output panel](#output-panel)
- [Frame strip](#frame-strip)
- [Source tab](#source-tab)
- [Pixels tab](#pixels-tab)
- [Palette tab](#palette-tab)
- [Paint tab and the paint editor](#paint-tab-and-the-paint-editor)
- [Export tab](#export-tab)
- [Project tab and .bitreel files](#project-tab-and-bitreel-files)
- [Unity integration](#unity-integration)
- [Keyboard shortcuts](#keyboard-shortcuts)
- [How it works](#how-it-works)
- [Limits and browser support](#limits-and-browser-support)
- [File formats](#file-formats)
- [Running locally and hosting](#running-locally-and-hosting)
- [Troubleshooting](#troubleshooting)
- [Credits and license](#credits-and-license)

---

## Features

- **Two conversion modes**
  - **Downscale** averages footage into blocks, for live action and 3D renders.
  - **Grid snap** detects the fake pixel grid in AI "pixel art style" video and recovers the art at its native resolution with crisp edges.
- **Loop tools**
  - **Loop detect** finds the cleanest loop anywhere in the clip and sets in and out points.
  - **Find loop point** keeps your in point and finds the best matching out point.
  - **Seam check** measures how cleanly the last frame wraps into the first.
- **Background keying** with an eyedropper, tolerance and edge choke. Fringe and blended edge colors are cleaned automatically in grid snap.
- **Palettes**
  - Built-in retro presets.
  - Palettes extracted from the source or from extracted frames.
  - Full per-swatch editing, a transparent swatch, and hex import and export.
- **Dithering**: none, Bayer 2×2, 4×4 and 8×8, Floyd-Steinberg, Atkinson.
- **Motion-aware flicker reduction**: stability, temporal smoothing and one-frame pop removal, all of which leave real motion alone.
- **Full-screen paint editor**
  - Pencil, eraser, restore, fill, picker and hand tools.
  - Zoom from ×1 to ×64, a pixel grid, onion skin and playback.
  - Undo and redo, and an option to paint all selected frames at once.
- **Frame selection**: include or exclude frames, select ranges, thin to every 2nd or 3rd frame.
- **Its own project format, `.bitreel`**: one file on your disk with the frames, every setting, painted edits and all saved versions. Save with Ctrl/⌘ + S.
- **Exports**
  - Animated GIF, MP4 or WebM, PNG frames, sprite sheet.
  - A Unity package with an editor importer that slices the sheet and builds a looping AnimationClip automatically.
- **Up to 60 fps** sampling and playback.

---

## Quick start

1. Open `bitreel.html` in a recent Chrome, Edge, Firefox or Safari, or use the hosted GitHub Pages demo.
2. Drop a video on the **Source** panel, or click **Choose video**.
3. BitReel checks the clip for a pixel grid.
   - If it finds one, it switches to **Grid snap** and tells you the cell size.
   - Otherwise it uses **Downscale**.
4. Drag on the source picture to set the crop.
5. Click **Loop detect** under the trim bar to find a clean loop, then press **▶ Play** to preview it.
6. In the **Source** tab, click **Extract frames**.
7. Adjust the look in **Pixels** and **Palette**. Fix anything by hand in the **Paint** editor.
8. Export from the **Export** tab.

The page opens with a procedural sample loop, so every tool can be tried before loading footage.

---

## Typical workflow

### Live action or renders (Downscale)

1. Load the video and crop to the subject.
2. Set the trim with **Loop detect** or the in and out points. Check it with **▶ Play**.
3. Pick a **Sample** rate (fps) in the Source tab and **Extract frames**.
4. In **Pixels**, choose an output **Width** (steps of 16), then tune contrast, saturation and dithering.
5. In **Palette**, choose a preset or rebuild a palette from the frames.
6. Turn on **Stability** if the result shimmers.
7. Check the **Seam** pill, then export.

### AI pixel-art video (Grid snap)

1. Load the clip. BitReel detects the grid and switches to **Grid snap** on its own when the grid is clear.
2. If the footage has a solid background, pick it with **Key out a color** *before* extracting. The fringe cleanup happens during extraction.
3. Check the grid overlay on the source. The lines should sit on the fake pixels. Adjust **Cell W / Cell H / Offset** if needed.
4. **Extract frames**. Frames come out at the native resolution, for example 91 × 100. Dither is set to None.
5. Use **Palette from source** to build a palette from the clip, skipping the background.
6. Clean up stray pixels in the paint editor, then export.

---

## Interface overview

```
┌──────────────────────────────┬──────────────────────────────┐
│ SOURCE                       │ OUTPUT                       │
│ video, crop, grid overlay    │ pixel-art loop preview       │
│ trim bar, Play, Loop detect  │ playback, seam check         │
├──────────────────────────────┴──────────────────────────────┤
│ Source · Pixels · Palette · Paint · Export · Project (tabs)  │
├─────────────────────────────────────────────────────────────┤
│ FRAMES: thumbnail strip with selection tools                │
└─────────────────────────────────────────────────────────────┘
```

![The BitReel interface](interface.jpg)

The **Source** and **Output** panels are always visible. Tools are grouped in tabs underneath. On narrow screens the panels stack.

The header shows the open project file (a dot means unsaved changes) and a status line that reports what each action did, along with any errors.

---

## Source panel

Shows the loaded video with the crop rectangle, the rule-of-thirds guides, and the pixel grid when Grid snap is on.

### Cropping

- **Drag on empty space** to draw a new crop.
- **Drag a corner handle** to resize.
- **Drag inside** the crop to move it.
- The aspect lock and **Full frame** reset are in the Source tab.

### Trim bar

| Control | What it does |
|---|---|
| Playhead slider | Scrubs the source. |
| **▶ Play / ■ Stop** | Plays the clip in real time between in and out, looping back to the in point. Shortcut: **P**. |
| **Set in / Set out** | Sets the in or out point to the playhead. Works during playback. |
| In / out fields | Type exact times in seconds. |
| **Find loop point** | Keeps the in point and moves the out point to the frame that best matches it. Click again to stop. |
| **Loop detect** | Searches start and end pairs for the cleanest loop and sets both points. Click again to stop. |

Playback stops on its own when you scrub, extract, or run a detection.

> The jump from out back to in requires a video seek, so a short hold at the wrap during source playback is normal. Extracted frames have no such pause. Use the Output panel's **Loop** or **Seam** modes to judge the real wrap.

### Loop detect candidates

After **Loop detect**, up to five candidates appear under the trim bar, for example `1. 2.67–4.17 s · 18 fr · 100%`.

- Click a candidate to apply it.
- Press **▶ Play** to preview it.

Each candidate shows its start and end, the frame count at your sampling rate, and how closely the end matches the start.

---

## Output panel

Shows the converted animation at the current settings. The header shows the conversion mode, the output size and the zoom, for example `grid snap · 91 × 100 · ×4`.

| Control | What it does |
|---|---|
| ◀︎ / ▶︎ | Steps one frame back or forward and pauses. |
| **Play / Pause** | Plays the selected frames. Shortcut: **Space**. |
| Frame readout | Position in the loop and the frame number. |
| **Loop** | Plays the selected frames in order, wrapping from last to first. |
| **Ping-pong** | Plays forward then backward. No seam. |
| **Seam** | Repeats only the frames around the wrap and shows the last and first frames side by side. |
| **FPS** | Playback rate, 1 to 60. Also used by every export. |
| **Onion** | Ghosts the previous frame over the current one. |
| Zoom | **Fit** (largest whole-number scale that fits), or ×1 to ×8. |
| Background | Checker, black or white, to judge transparency. |

### Seam pill

Compares the change between the last and first selected frames with the typical change between consecutive frames:

- **Clean loop** means the wrap is no bigger than a normal step.
- **Visible seam** means it's up to 3× a normal step.
- **Seam jump** means it's larger than that.

### Empty state

When a new video is loaded, the previous output is cleared. The Output panel then shows **No frames yet**, with a button to the Source tab. Your settings are kept.

---

## Frame strip

A thumbnail of every extracted frame, with its index and source time.

- **Click** a frame to include or exclude it from the loop. Excluded frames are dimmed and struck through.
- **Shift-click** applies the same state to the range since your last click.
- **Double-click** opens the frame in the paint editor.
- A dot after the frame number marks a painted frame.
- **All / None / Invert / Every 2nd / Every 3rd** change the selection in bulk.

Only selected frames are played, seam-checked, flicker-reduced and exported.

---

## Source tab

### Crop

- **Aspect lock**: Free, 1:1, 4:3, 16:9, 9:16, 2:1.
- **Full frame** resets the crop.

### Key out a color

Makes a background color transparent.

1. Click **Pick color**, then click the background in the source picture. A 3×3 area is averaged.
2. Adjust the two sliders:
   - **Tolerance** (1 to 100) sets how far from the picked color still counts as background.
   - **Choke** (0 to 3 px) grows the transparent area to trim edge pixels.
3. Use **On / Off** to toggle the key without losing the color.

How each mode treats the key:

- **Downscale**: keyed pixels are left out of each block's average, so background color doesn't tint edges. A block becomes transparent when more than half of it is keyed.
- **Grid snap**: keyed pixels and *fringe* are left out when choosing each cell's color. Fringe means colors near the key, or with the key's hue at any brightness, such as magenta blended with a black outline. A cell that is mostly background becomes transparent.

Set the key before extracting in Grid snap mode, because the cleanup happens during extraction.

### Palette from source

Builds a palette directly from the video.

- **Colors**: 2 to 64.
- **From**: the frame on screen, or 10 frames spread between in and out.

The crop, the current Pixels adjustments and the key are all applied first. Background and fringe colors don't use up palette slots.

### Loop detect settings

- **Length**: shortest and longest loop in seconds. Default 0.8 to 6 s.
- **Search**: the whole video, or only between the current in and out.

### Conversion

- **Downscale**: box-averages the crop down to the output width. For live action and renders.
- **Grid snap**: for footage that already looks like pixel art. Recovers one clean color per fake pixel at the native resolution.

#### Grid snap controls

| Control | What it does |
|---|---|
| **Detect grid** | Measures the grid from this frame, or from this frame plus 5 frames between in and out. |
| **Show grid** | Toggles the grid overlay on the source. |
| **Cell W / Cell H** | Cell size in source pixels, from 1.5 to 128. Fractional sizes like 7.2 are normal for AI video. |
| **Offset X / Y** | Where the first cell starts inside the crop. |
| **Square cells** | Locks Cell H to Cell W. |
| **Track per frame** | Re-aligns the grid on each frame when it clearly moved, and snaps single lines to strong edges that agree with their neighbors. Steady footage stays on the detected grid. |
| Readout | Native size, for example `91 × 100 native px`, and how regular the grid is. |

When a video loads, BitReel samples three frames. If it finds a clear grid between 3 and 48 px, it switches to Grid snap. Otherwise it uses Downscale. The status line says which one it chose. The mode is remembered between sessions.

### Frames

- **Sample**: 6, 8, 10, 12, 15, 24, 25, 30, 48, 50 or 60 fps.
- The estimate shows the frame count and the working or native size.
- **Extract frames · downscale / grid snap**: the label always shows which mode will run.

Extraction samples frames at `in + k / fps` and stops before the out point. The output FPS is set to the sampling rate.

---

## Pixels tab

### Size and tone

| Control | Range | Notes |
|---|---|---|
| **Native grid resolution** | on/off | Only after a grid-snap extraction. Uses the recovered size exactly. |
| **Width** | 16 to 400, steps of 16 | Height follows the crop's aspect ratio. Crop to 1:1, 16:9 or 2:1 for clean numbers. |
| **Brightness** | -100 to 100 | |
| **Contrast** | -100 to 100 | Default 10. |
| **Saturation** | -100 to 100 | Default 10. |
| **Gamma** | 0.50 to 2.00 | |

### Dither

- **None** gives flat color areas. Best for native pixel art.
- **Bayer 2×2, 4×4, 8×8** are ordered patterns. They stay fixed from frame to frame, so they don't shimmer. Default is 4×4.
- **Floyd-Steinberg, Atkinson** are error diffusion. They look finer on stills but shimmer in motion.
- **Strength** goes from 0 to 100.

### Flicker

The pill shows the share of pixels that change color per frame, before and after reduction, for example `6.5% → 4.1% change/frame`.

| Control | What it does |
|---|---|
| **Stability** (0 to 100, default 35) | A pixel keeps its previous color while it still matches closely, but only if the footage under it barely changed. Real motion updates normally, so moving subjects don't smear. |
| **Smoothing** (0 to 100, default 0) | Blends each frame with its neighbors before conversion. Useful for grainy footage. Softens fast motion. |
| **Remove one-frame pops** (default on) | Clears a pixel that flashes to another color for a single frame, but only when the source agrees it was noise. |

All three work on the selected frames as a loop, so the wrap is stabilized too.

---

## Palette tab

### Colors

- **Presets**:
  - PICO-8 (16)
  - Sweetie 16 (16)
  - Game Boy (4)
  - C64 (16)
  - CGA 4 (4)
  - Grayscale 8 (8)
  - 1-bit (2)
  - Custom
- **Swatches**: click to select one.
- **Swatch editing**:
  - The color picker and hex field edit the selected swatch.
  - **Transparent** marks it as transparent in exports and the preview.
  - **Remove** deletes it. At least 2 colors remain.
  - **Add** duplicates it. Up to 255 colors.

Any edit switches the preset to **Custom**. Palettes are remembered between sessions.

### Rebuild

- **From extracted frames**: median cut plus k-means on the selected frames, using 2 to 64 colors. Keyed pixels are skipped.
- **Sort by light** orders swatches by perceptual lightness (OKLab L).

### Hex list

Paste any text that contains 6-digit hex colors (`1a1c2c 5d275d …`) and click **Apply list**. **Copy** copies the current palette as hex.

Colors are matched in the OKLab color space, so the nearest palette color is the one that looks closest, not just the one with the closest RGB numbers.

---

## Paint tab and the paint editor

Open the editor with **Open paint editor** or by double-clicking a frame in the strip. It opens full screen on the frame shown in the Output panel.

### Top bar

| Tool | Key | What it does |
|---|---|---|
| **Pencil** | B | Paints with the selected palette color. |
| **Eraser** | E | Paints transparent. |
| **Restore** | R | Returns pixels to their converted color, removing edits. |
| **Fill** | G | Floods a connected area of the same color. |
| **Pick** | I | Picks a color from the frame. Picking a transparent pixel switches to Eraser. |
| **Hand** | H | Pans the view. |

- The color chip shows the current color.
- **Brush size**: 1, 2, 3, 4 or 6 px.
- **All selected frames**: each stroke or fill applies to every selected frame. Good for fixing something static.
- **Undo / Redo** keep up to 80 steps.
- **Clear edits** removes all edits on the current frame, or on the selected frames when *All selected frames* is on. You can undo it.
- **Done** closes the editor. **Esc** works too.

### Bottom bar

- **▶ Play / ■ Stop** plays the animation inside the editor at the output FPS, following Loop or Ping-pong. Painting, stepping or closing stops it. Shortcut: **P**.
- **◀︎ / ▶︎** step frames. The arrow keys do the same.
- **Onion** ghosts the previous frame.
- **Grid** shows the pixel grid from ×6 zoom up.
- **Palette strip** sets the color with one click.
- **Zoom**: **−**, **+** and **Fit**. The mouse wheel zooms at the cursor.
- **Cell readout** shows the coordinates of the pixel under the cursor.

### Navigation

- **Wheel** zooms around the cursor, from ×1 to ×64.
- **Shift + wheel** pans horizontally.
- **Space + drag**, **middle mouse drag** or the **Hand** tool pans freely.

### How edits are stored

- Edits are stored as **colors**, not palette slots. They survive palette changes, dither changes, flicker settings and palette rebuilds. Each painted color is re-matched to the current palette.
- Edits are kept **per output size**. Changing the width hides them until you return to that size.
- Painted frames are marked in the frame strip and listed in the Unity manifest.
- Edits are saved in versions and project files.

---

## Export tab

### Settings

| Setting | Applies to | Notes |
|---|---|---|
| **Name** | All | Used for file names. |
| **Scale** | GIF, video, PNG frames, sprite sheet | 1, 2, 4, 6 or 8×, nearest-neighbor. Unity exports are always 1:1. |
| **Pixels / unit** | Unity | Default 16. |
| **Pivot** | Unity | Bottom center, center or bottom left. |
| **Sheet columns** | Sheets, Unity | 0 means automatic (√n). |
| **Padding** | Sheets, Unity | 0, 1 or 2 px between frames. |
| **Video loops** | Video | 1, 4, 8 or 16 repetitions. |

All exports use the **selected** frames, in order, at the output **FPS**. Ping-pong mode also plays frames back in reverse in GIF, video and Unity exports.

### Formats

| Button | Output |
|---|---|
| **Unity package (.zip)** | Sprite sheet, manifest, single frames, palette and editor importer. See [Unity integration](#unity-integration). |
| **Sprite sheet .png** | All frames in a grid at the chosen scale. |
| **PNG frames .zip** | One PNG per frame at the chosen scale. |
| **Animated .gif** | Exact palette with no re-quantization. Loops forever. Transparent pixels stay transparent. |
| **Video** | MP4 (H.264) where the browser can record it, otherwise WebM. Recorded in real time. Transparent pixels become black. |

> **GIF timing:** GIF delays are in hundredths of a second, and browsers slow down delays under 2/100 s. GIFs therefore play at most 50 fps. Video, PNG and Unity exports keep the full FPS.

---

## Project tab and .bitreel files

BitReel saves your work in its own file format, **`.bitreel`**, on your computer. It doesn't depend on any account, server or browser storage. A project file holds everything needed to pick the work up again, including every saved version.

### Project file

| Control | Shortcut | What it does |
|---|---|---|
| **Project name** | | Used as the file name. The header shows the file, with a dot while there are unsaved changes. |
| **Save** | Ctrl/⌘ + S | Writes the project. The first save asks where. After that, Save writes back to the same file. |
| **Save as…** | Ctrl/⌘ + Shift + S | Writes a copy under a new name or location. |
| **Open…** | Ctrl/⌘ + O | Opens a `.bitreel` file. You can also drop one anywhere on the page. |
| **New** | | Starts over. Your settings are kept. |

How saving works depends on the browser:

- **Chrome and Edge** write straight to the file on disk through the File System Access API, like a desktop app.
- **Firefox and Safari** download the file. Save again to download an updated copy.

Opening, New and closing the tab warn you when there are unsaved changes. Opening and New ask for a second click.

### What a project contains

- the extracted frames
- the crop and trim (in, out, sampling rate)
- the conversion mode and grid settings
- the key
- Pixels and flicker settings
- the palette and transparent swatch
- the frame selection
- painted edits
- export settings
- every saved version

The **source video is not stored**, which keeps project files small. Load the video again if you want to re-crop or re-extract. If you load the same file name, the crop and trim are restored too.

### Versions

A version is a named snapshot kept **inside the project file**.

- **Save version** adds a snapshot of the current state. Name it or keep the automatic name. Press Enter to save.
- Each version shows:
  - a thumbnail
  - the time
  - the frame count and conversion mode
  - the number of colors and painted frames
- **Load** restores that version. **Delete** removes it. Both ask for a second click.
- Versions become permanent when you save the project.

Versions that share the same extracted frames share one copy of those frames in the file. Ten versions with different palettes cost little more than one.

Older `.zip` project files from earlier builds (Pixelbench and BitReel) still open.


---

## Unity integration

### Package contents

```
name_unity.zip
├── name/
│   ├── name_sheet.png            sprite sheet, 1:1 pixels
│   ├── name.pixelsheet.json      manifest
│   ├── name_palette.png          palette, N×1
│   ├── name.hex                  palette as hex, one color per line
│   └── frames/
│       ├── name_000.png
│       └── …
├── Editor/
│   └── PixelSheetImporter.cs     editor importer (install once)
└── README.md                     import notes
```

### Importing

1. **Install the importer once.** Copy `Editor/PixelSheetImporter.cs` to `Assets/Editor/PixelSheetImporter.cs`.
   - Skip this if it's already in the project. Two copies won't compile.
   - It needs the **2D Sprite** package (`com.unity.2d.sprite`), which ships with the 2D templates.
2. **Copy the `name/` folder** anywhere under `Assets/`, for example `Assets/Art/Animations/name/`.
3. The importer runs on its own when the `.pixelsheet.json` file is imported. It:
   - sets the sheet to Sprite (Multiple), Point filter, no compression, no mipmaps, clamp wrap, and the manifest's pixels per unit
   - slices one sprite per frame, named `name_000`, `name_001`, …, with the manifest's pivot (it keeps sprite IDs stable on re-import)
   - creates or updates `name.anim`, a looping clip on `SpriteRenderer.m_Sprite` at the manifest FPS, including ping-pong
   - creates `name.controller` once, with the clip as its default state

### Re-running the importer

- From the menu: **Tools › BitReel › Import Selected Sheet**, with the JSON selected.
- From code:
  ```csharp
  PixelSheetImporter.Import("Assets/Art/Animations/name/name.pixelsheet.json");
  ```
  The console logs `[BitReel] Imported name: N sprites at F fps -> path`.

### Using it with Claude and the Unity MCP

The package is laid out so an assistant with Unity MCP access can import it unattended:

1. Check whether a `PixelSheetImporter` class already exists. If not, create the script and wait for compilation.
2. Copy the folder into `Assets/` and refresh the AssetDatabase.
3. If no clip appears, call `PixelSheetImporter.Import(path)` through code execution and check the console for the `[BitReel] Imported` line.
4. To place the animation in a scene, create a GameObject with a `SpriteRenderer` (sprite `name_000`) and an `Animator` that uses `name.controller`.

### Manual import (without the importer)

1. Texture Type **Sprite (2D and UI)**, Sprite Mode **Multiple**, Filter **Point**, Compression **None**, mipmaps off.
2. Slice by **Cell Size** using the manifest's `frameWidth × frameHeight`, offset 0, padding as in the manifest.
3. Build an AnimationClip at the manifest FPS from the sprites in order. For ping-pong, follow them with the same sprites in reverse, leaving out the first and last.

---

## Keyboard shortcuts

### Main page

| Key | Action |
|---|---|
| **Space** | Play or pause the output |
| **← / →** | Previous or next output frame |
| **P** | Play or stop the source loop (between in and out) |
| **Ctrl/⌘ + S** | Save the project |
| **Ctrl/⌘ + Shift + S** | Save the project as… |
| **Ctrl/⌘ + O** | Open a project |
| **← / →** on the tab bar | Switch tabs |

### Paint editor

| Key | Action |
|---|---|
| **B / E / R / G / I / H** | Pencil, eraser, restore, fill, pick, hand |
| **+ / − / 0** | Zoom in, zoom out, fit |
| **Wheel** | Zoom at the cursor |
| **Shift + wheel** | Pan horizontally |
| **Space + drag**, middle mouse | Pan |
| **← / →** | Previous or next frame |
| **P** | Play or stop the animation |
| **Ctrl/⌘ + Z** | Undo |
| **Ctrl/⌘ + Shift + Z**, **Ctrl/⌘ + Y** | Redo |
| **Esc** | Close the editor |

---

## How it works

### Pipeline

```
video ─▶ crop ─▶ sample frames ─▶ [Downscale | Grid snap] ─▶ adjust (brightness, contrast,
saturation, gamma) ─▶ key mask ─▶ quantize to palette (OKLab) + dither ─▶ flicker reduction
─▶ painted edits ─▶ preview / export
```

- **Work frames.** Downscale mode stores frames at up to 400 px on the long side. Grid snap stores them at their native size.
- **Live updates.** Changing a setting re-runs only the stages after it, so updates feel immediate.

### Downscale

Each output pixel is the area average of its block of source pixels. Keyed pixels are left out of the average, so background color doesn't bleed into edges.

### Grid detection

1. Edge strength is summed along each axis to build a profile whose peaks sit on cell boundaries.
2. A high-pass filter removes smooth gradients.
3. Candidate periods from 2 to 64 px are scored by spectral strength. Each sampled frame is scored on its own, so a grid that shifts between frames doesn't blur the measurement.
4. Harmonics (p/2, p/3) score as high as the true period, so the largest strong period wins. It is then halved while the in-between lines still carry real edges, which handles art whose outlines mostly fall on every other line.
5. The period is refined to 0.002 px, and the offset is fitted directly to the edge lines.
6. Each axis keeps its own measurement. AI footage is often slightly non-square, and averaging the two would drift over many rows.

### Grid snap extraction

- **Per-frame alignment.** With *Track per frame* on, a frame's grid shifts only if that clearly fits better than the detected offset. Single lines move only onto strong edges that agree with their neighbors.
- **Cell color.** Each cell's color is the **dominant** color in the middle 50% of the cell, using 4-bit-per-channel buckets and averaging the winning bucket. Blur and compression around the edges don't leak in.
- **Keying.** Keyed and fringe pixels are left out of the vote. Mostly-background cells become transparent.

### Quantization and dithering

- Colors are matched to the palette in **OKLab**, a perceptual color space, with a 262,144-entry lookup cache.
- Bayer dithering adds a fixed threshold pattern before matching.
- Floyd-Steinberg and Atkinson diffuse the error in serpentine order.

### Flicker reduction

- **Stability** keeps a pixel's previous palette color when the dithered source is within the threshold of it *and* the source color under that pixel barely changed between frames. Motion is never held back.
- It runs twice around the loop, so the first frame is stabilized against the last.
- **Pop removal** only reverts a one-frame change when the source at that pixel was steady across all three frames.

### Loop detect

1. Frames are sampled at the sampling rate, capped at about 720 samples, as 24 px-wide color thumbnails.
2. Every start and end pair within the length limits is scored as the seam difference divided by the average motion inside the loop, with a slight preference for longer loops.
3. Stretches with almost no motion are skipped, because they loop trivially.
4. The top five distinct candidates are snapped to the frame grid, and the end is re-checked at the full sampling rate.

### Seam metric

The share of pixels whose palette index differs between the last and first selected frames, compared with the median frame-to-frame change.

### GIF encoder

A built-in GIF89a encoder writes the exact palette (padded to a power of two), with LZW compression, NETSCAPE looping and a transparency index. Keyed or erased pixels get their own slot when no palette color is marked transparent.

---

## Limits and browser support

| Limit | Value |
|---|---|
| Frames per extraction | 480 (8 s at 60 fps) |
| Downscale working size | 400 px on the long side |
| Grid snap native size | up to 640 × 640 cells |
| Output width | 16 to 400 px, steps of 16, or native in grid snap |
| Palette | 2 to 255 colors |
| Sampling and playback | up to 60 fps (GIF plays at most 50 fps) |

- **Video formats.** Anything the browser can decode: MP4 (H.264), WebM (VP8/VP9), MOV where supported. Some Chromium builds can't decode H.264. Convert to WebM if a file won't load.
- **Video export.** Uses `MediaRecorder` and records in real time. MP4 where the browser supports recording it (recent Chrome and Edge), otherwise WebM.
- **Browsers.** Tested in Chromium. Recent Firefox and Safari should work. Some features (WebP thumbnails in versions, MP4 recording) fall back automatically.

---

## File formats

### `name.pixelsheet.json` (Unity manifest)

```json
{
  "format": "bitreel.pixelsheet",
  "version": 1,
  "name": "runner",
  "image": "runner_sheet.png",
  "sheetWidth": 455, "sheetHeight": 400,
  "frameWidth": 91, "frameHeight": 100,
  "columns": 5, "rows": 4, "padding": 0,
  "frameCount": 18,
  "fps": 12,
  "loopMode": "loop",
  "pixelsPerUnit": 16,
  "pivotX": 0.5, "pivotY": 0,
  "filterMode": "Point",
  "compression": "None",
  "createAnimator": true,
  "transparentIndex": -1,
  "palette": ["#1a1c2c", "…"],
  "key": { "color": "#ff2dfd", "tolerance": 22, "choke": 0 },
  "paintedFrames": [3, 7],
  "coordinateOrigin": "top-left",
  "frames": [
    { "index": 0, "x": 0, "y": 0, "w": 91, "h": 100, "time": 0,
      "sourceFrame": 0, "sourceTime": 2.67 }
  ],
  "source": { "file": "112.mp4", "crop": { "x": 351, "y": 0, "w": 658, "h": 720 },
              "in": 2.67, "out": 4.17, "sampleFps": 12 },
  "conversion": { "mode": "grid-snap", "cellWidth": 7.198, "cellHeight": 7.186,
                  "offsetX": 1.1, "offsetY": 0.7 },
  "settings": { "…": "pixel, dither and flicker settings" },
  "createdWith": "BitReel"
}
```

- Frame rects use a **top-left** origin. The importer converts them to Unity's bottom-left origin.
- `loopMode` is `loop` or `pingpong`.
- `key` is `null` when no key is active.
- `conversion.mode` is `downscale` or `grid-snap`.

### `name.bitreel` (project file)

A `.bitreel` file is a ZIP container with this layout:

| Entry | Contents |
|---|---|
| `mimetype` | The text `application/x-bitreel`. Always the first entry and stored uncompressed, so tools can identify the file. |
| `project.json` | Project metadata, the current state and every saved version. |
| `frames/<id>.png` or `.webp` | Frame atlases: all extracted frames of one extraction packed in a grid. Grid-snap and small sets are lossless PNG, large downscale sets are WebP at quality 0.95. |
| `thumbs/current.png`, `thumbs/<versionId>.png` | Thumbnails. |

`project.json`:

```json
{
  "format": "bitreel",
  "formatVersion": 1,
  "app": "BitReel",
  "name": "runner",
  "created": "2026-10-22T14:02:11.000Z",
  "modified": "2026-10-22T14:20:45.000Z",
  "current": { "thumb": "thumbs/current.png", "state": { "…": "a state, see below" } },
  "versions": [
    { "id": "vmvjmjou2", "name": "gameboy palette", "createdAt": "2026-10-22T14:15:03.000Z",
      "thumb": "thumbs/vmvjmjou2.png", "state": { "…": "a state" } }
  ]
}
```

A **state** holds:

| Fields | Contents |
|---|---|
| `atlasId`, `atlasType`, `cols` | Which atlas in `frames/` holds this state's frames, and its grid columns. |
| `frameCount`, `workW`, `workH`, `times`, `on` | Frame count and size, source time of each frame, and the selection. |
| `out` | Pixels, dither and flicker settings, including `native`. |
| `pal`, `presetName`, `trans` | Palette as hex, preset name, transparent swatch index (`-1` for none). |
| `key` | `on`, `rgb`, `tol`, `choke`. |
| `conv`, `native`, `grid` | Conversion mode, whether frames are grid-snapped, and the grid (`w`, `h`, `x`, `y`, `sq`, `track`, …). |
| `play`, `exp` | Output FPS and loop mode, and export settings. |
| `source` | Source file name, crop, in and out, sampling rate. |
| `edits` | Painted edits, keyed by output size (`"91x100"`) and frame index. Each is a flat `[pixelIndex, value, …]` list, where `value` is `0xRRGGBB` for a color or `-2` for transparent. |

Atlas ids are a hash of the frame data plus the size, for example `81aa797f-320x180x24`, so identical extractions are stored once.


---

## Running locally and hosting

BitReel is a single HTML file with no build step.

- **Locally.** Open `bitreel.html` in a browser. Some browsers restrict file features on `file://`, so a local server is the safest option:
  ```bash
  python3 -m http.server 8000
  # then open http://localhost:8000/bitreel.html
  ```
- **GitHub Pages.** Enable Pages for the repository, serving from the root or `/docs`, and open `https://<user>.github.io/<repo>/bitreel.html`. Renaming the file to `index.html` makes it the site's landing page.
- **External dependencies.**
  - **JSZip** from cdnjs, for the zip exports and project files.
  - **Google Fonts**: Silkscreen, IBM Plex Sans, IBM Plex Mono. They fall back to system fonts when offline.
  - For a fully offline build, vendor JSZip next to the HTML and change the script tag.
- **Privacy.** Videos and frames stay in your browser, and projects are files on your own disk. Nothing is uploaded.

---

## Troubleshooting

**The output looks blurry or mushy on pixel-art footage.**
The clip was converted with **Downscale**. Check the Output header or the Extract button for the mode. Switch to **Grid snap** in the Source tab, click **Detect grid**, check the overlay, and extract again.

**Grid lines drift off the fake pixels toward one side.**
The cell size is slightly off. Run **Detect grid** on *in to out* rather than a single frame, or adjust Cell W / Cell H by a few hundredths until the overlay stays on the pixels across the whole crop.

**The detected cell is twice the real size.**
This can happen when most outlines fall on every other line. Halve **Cell W / Cell H** by hand.

**Colored specks or a halo around the subject.**
- Set the key **before** extracting in Grid snap.
- Raise **Tolerance**, and use **Choke** 1 in Downscale.
- Rebuild the palette with **Palette from source** so background colors don't use up slots.

**Moving parts smear or trail.**
Lower **Stability** or turn off **Remove one-frame pops**. Both are motion-aware, but very fast motion at low frame rates can still trigger them.

**Visible jump at the loop point.**
- Use **Loop detect**, or **Find loop point**.
- Check the **Seam** view.
- Remove a frame at the end with the frame strip.
- Use **Ping-pong** for motion that can play in reverse.

**A video won't load.**
The browser can't decode that codec. Convert it to WebM or H.264 MP4.

**Exports or project files don't download.**
The page is probably running in an embedded view that blocks downloads. Open it in a regular browser tab. Inside Claude, files go through Claude's save dialog instead, and project files arrive as `.bitreel.zip`, which **Open…** accepts as-is.

**Save asks where to save every time.**
Your browser doesn't support writing back to a file (Firefox, Safari), so each save downloads a fresh copy. In Chrome or Edge, the first Save picks the file and later saves update it.

---

## Credits and license

Created by Luc Leclerc, ATV Studio, Montreal.

Grid-snap reconstruction was written from scratch. [pixel-bench](https://github.com/Retro-Diffusion/pixel-bench) by Retro Diffusion, an open benchmark for pixel-art reconstruction, informed the testing approach.

License: see `LICENSE`.
