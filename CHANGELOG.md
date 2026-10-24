# Changelog

All notable changes to BitReel are listed here, newest first.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and versions follow [Semantic Versioning](https://semver.org/):

- **Major** (2.0.0): a change that breaks existing projects or exports.
- **Minor** (1.7.0): a new feature that keeps everything working.
- **Patch** (1.6.1): a fix only.

Versions before 1.2.0 were numbered after the fact, from the order the features were built. They are dated by month only.

## [Unreleased]

## [1.6.0] - 2026-10-24

### Added
- **Game tiles** (Source tab): pick a tile size (8 to 128 px) and fit the character to your game's tile grid.
  - Cyan tile lines over the source. In Grid snap, the pixel grid fades under them.
  - **Pixel size** scales the video against the tiles. **Match grid** keeps the art's own pixels.
  - **Fit height to N tiles** measures the character in the current frame (dashed yellow box), scales it to exactly N tiles tall, and frames it with the feet on a tile line.
  - Resizing the crop snaps it to whole tiles. Extraction outputs exactly whole tiles (for example 64 × 64 for 2 × 2 tiles of 32).
  - Turning tiles on sets Unity Pixels / unit to the tile size, so one tile is one Unity unit.
  - With Trim to content on, the exported frame grows to whole tiles, with the feet on the bottom edge.
- Version number next to the logo, and `appVersion` in saved project files.

### Changed
- In Grid snap, moving the crop keeps the pixel grid locked to the video instead of moving it with the crop.
- Grid-snapped frames resized to a different scale pick pixels instead of averaging them, so edges stay crisp.

## [1.5.0] - 2026-10-23

### Added
- **Skip blended in-between frames** (Frames group, on by default). Frame-rate conversions and some AI exports add cross-faded in-between frames that show up as ghosts and double outlines. BitReel probes each new video once and, when it finds blends, replaces any sample that lands on one with the nearest real frame. The status line reports how many were replaced.

## [1.4.1] - 2026-10-23

### Fixed
- The Source, Output and States panels are always the same height. The States list scrolls inside its panel instead of making the row taller.
- The Create animations button and the States column no longer overflow on narrow columns.

## [1.4.0] - 2026-10-23

### Added
- Unity export: **Include: Selected animations**, with a checklist of the set and All / None buttons. Selections are saved in the project.

### Changed
- The default Unity state is the set's first animation when it is exported, otherwise the first exported one.

## [1.3.1] - 2026-10-23

### Fixed
- **Create animations** always uses the Conversion type selected in the Source tab (Downscale or Grid snap), with the current crop and grid, for every animation it makes.

### Changed
- States that already have an animation are re-extracted in place instead of duplicated. The button reads **Update animations** and shows the conversion it will use.
- Grid snap without a grid set stops with a message instead of creating anything.

## [1.3.0] - 2026-10-23

### Added
- **States column** to the right of the Output panel: click a state to loop its source range and show its animation in the Output, for side-by-side comparison.
- Step through states with the ↑ ↓ buttons or Alt+↑ / Alt+↓. **Play on select** option.
- **Merge with…** menu on each state to merge it with any other state, including the ones in between.

## [1.2.0] - 2026-10-23

### Added
- **Detect states**: splits a clip that chains several actions (idle, run, jump, hit) into states and transitions.
  - Colored bands, best-loop marks and hatched transitions on the trim bar, plus chips under it.
  - Rename, check or uncheck each state and transition, and create every checked one as an animation in one click.
  - Repeats of an action are grouped. Names are guessed (idle, run, jump, action N).
  - Sensitivity setting: Fewer, Normal or More states.
- **Plays once** setting per animation for one-shot actions and transitions. Unity clips for those don't loop (`loopMode: "once"`).

## [1.1.0] - 2026-10

### Added
- **Animation sets**: several animations (idle, run, jump) in one project, sharing the palette, key and pixel settings. One Unity Animator Controller per set, with one state per animation.
- **Stabilize**: lock the character in place horizontally and pin the feet, for walk and run cycles that should play on the spot.
- **Trim to content** with an optional shared frame size and anchor across all animations, so switching animations doesn't make the sprite jump.
- Project format 2 for animation sets. Older projects still open.

### Fixed
- Undo in the paint editor (a name clash with project versions broke it).

## [1.0.0] - 2026-10

First public release, open source under the MIT License.

### Added
- Own project file format, `.bitreel` (a zip with the project, frames and saved versions). Save and Open use the browser's file dialogs.
- README with full documentation and an interface screenshot. LICENSE.md (MIT).

### Changed
- Exports and saves download straight from the browser. Nothing is stored in Claude.

### Fixed
- Export buttons that stayed disabled.

## [0.7.0] - 2026-10

### Fixed
- Extraction quality: the selected conversion mode is remembered, grid snap is detected automatically on load when the footage clearly is pixel art, and the Extract button and Output header always show which mode runs.
- Grid drift on long clips: each axis keeps its own cell size, with per-frame tracking.
- Flicker reduction smearing moving characters: it is now motion-aware.
- Magenta fringe around keyed characters in grid snap: keying is hue-aware.
- Flat-colored clips wrongly detected as pixel art.

### Changed
- Loading a new video clears the existing output.

## [0.6.0] - 2026-10

### Added
- **Grid snap** conversion for AI "pixel art style" video: detects the fake pixel grid and keeps one clean color per cell at the native resolution.
- **Loop detect** in the Source panel: finds the cleanest loop in the clip and sets in and out.
- Play / stop for the source loop, and for the animation in the paint editor.

### Changed
- Renamed from Pixelbench to **BitReel**.
- Source and Output panels always visible side by side, with the tools in tabs underneath.

## [0.5.0] - 2026-10

### Added
- Tabbed interface.
- Saved versions of the work in progress.
- Full-screen paint editor with zoom.
- Sampling and playback up to 60 fps.

## [0.4.0] - 2026-10

### Added
- Palette extracted from the source, with a chosen number of colors.
- Flicker reduction.
- Output width in steps of 16 px.

### Changed
- Find loop point moved to the Source panel.

## [0.3.0] - 2026-10

### Added
- Key color picker on the source for transparency.
- Paint editing of individual frames.

## [0.2.0] - 2026-10

### Added
- Unity export: sprite sheet, JSON manifest, single frames, palette and an editor importer script, ready to import with Claude and the Unity MCP.

## [0.1.0] - 2026-10

### Added
- First version: video upload, crop, frame selection, palette editing and loop testing.

[Unreleased]: https://github.com/abovethevoid/bitreel/compare/v1.6.0...HEAD
[1.6.0]: https://github.com/abovethevoid/bitreel/releases/tag/v1.6.0
