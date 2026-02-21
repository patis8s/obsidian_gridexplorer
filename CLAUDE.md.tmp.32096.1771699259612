# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this directory is

This is the **installed plugin directory** for the GridExplorer Obsidian plugin (v2.9.23 by Devon22), located inside an Obsidian vault's `.obsidian/plugins/` folder. It contains **compiled artifacts only** — not the TypeScript source.

| File | Purpose |
|---|---|
| `main.js` | Bundled plugin (esbuild output from `src/`) — the only executable code |
| `styles.css` | All plugin CSS |
| `manifest.json` | Plugin metadata (id, version, minAppVersion) |
| `data.json` | Persisted user settings (loaded/saved by Obsidian's plugin API) |

The original TypeScript source lives on GitHub (Devon22/gridexplorer). Editing `main.js` directly is the only way to modify behavior in this directory.

## Architecture (inferred from bundle)

The bundle is assembled from these source modules (visible as esbuild comments in `main.js`):

- **`src/main.ts`** — Plugin entry point; registers the `GridExplorerPlugin` class, commands, ribbon icons, and settings tab
- **`src/GridView.ts`** — The main `ItemView` leaf that renders the grid of cards; handles modes, sorting, file watching, and the note-in-grid overlay
- **`src/ExplorerView.ts`** — The sidebar tree view (folder hierarchy + mode list + stash group)
- **`src/renderHeaderButton.ts`** — Renders the header bar buttons (mode selector, sort, refresh, search, etc.)
- **`src/utils/fileUtils.ts`** — Vault file helpers (filtering, sorting, frontmatter reading, image extraction)
- **`src/translations.ts`** — i18n via `t(key)` function; supports `en` and `zh-TW`

### Key design patterns

**Two-panel layout**: ExplorerView (left sidebar leaf) + GridView (main/tab leaf). They communicate via plugin-level state and Obsidian's event system.

**Browse modes**: Folder, Bookmarks, Search, Backlinks, Outgoing Links, All Files, Recent Files, Random Note, Tasks, and user-defined **Custom Modes**. Custom modes execute DataviewJS code (via the Dataview plugin API) to return a file list.

**Settings** (`data.json`): All settings are read at render time from `this.plugin.settings`. Grid dimensions are applied as CSS custom properties (`--grid-item-width`, `--grid-item-height`, `--image-area-width`, `--image-area-height`, `--title-font-size`) on the container element so CSS can reference them.

**Card layouts**: Horizontal card (default) uses `position: absolute` for the image area and CSS float tricks so body text wraps around the thumbnail. Vertical card uses flexbox `order` to place the image top or bottom.

### CSS conventions

- All classes are prefixed `ge-` (e.g., `.ge-grid-item`, `.ge-folder-item`, `.ge-tag`)
- Media file type icons use modifier classes: `.ge-img`, `.ge-video`, `.ge-audio`, `.ge-pdf`, `.ge-canvas`, `.ge-base`
- Color theming for folders/notes uses `.ge-folder-color-{color}` / `.ge-note-color-{color}` classes with Obsidian CSS variable `rgba(var(--color-{color}-rgb), 0.2)` patterns
- Mobile responsive overrides use `.is-mobile` and `.is-tablet` selectors
