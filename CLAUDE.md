# CLAUDE.md

## What this is

A single-file local JSON viewer. Everything lives in `index.html` — no build step, no dependencies, no server.

## Architecture

`index.html` has three sections:
- **CSS** — Catppuccin-based theme tokens (default: Latte/light), split-pane layout, tree node styles
- **HTML** — header, tab bar, two-panel body (input+filter left, tree+location right), footer path bar, floating tooltip, shortcuts overlay
- **JS** — tree rendering, search, filter (DeepPick), path tracking, tabs, location panel, themes

## Layout

- **Header**: app title, theme picker, location panel toggle
- **Tab bar**: multi-document tabs (add, close, rename, switch); state persisted in `localStorage`
- **Left panel**: JSON input textarea (top) + DeepPick filter textarea (bottom, resizable), Parse / Format / Clear buttons
- **Right panel**: tree toolbar (search, Expand All, Collapse All, Depth 2) + tree scroll area + optional Location sidebar
- **Footer path bar**: shows hovered node's JSON path; pins on click (or ⌘); pinned path can be copied; DeepPick schema shown alongside
- **Floating tooltip**: near-cursor overlay showing `path` and `dp` values; click row to copy when pinned

## Key conventions

- All tree rendering is pure DOM (`createElement` / `textContent`), never `innerHTML` with user data
- Expand/collapse state is CSS-driven: `.node.collapsed > .kids { display: none }` — JS only toggles the `collapsed` class
- `build(val, key, isLast)` — `key` is `null` for array items and the root node; `isLast` controls trailing comma
- String values over 280 chars are truncated with an inline expand toggle; control characters (`\n`, `\t`, etc.) are escaped for display
- Theme tokens live in a `THEMES` object (keys: `latte`, `mocha`, `light`, `dracula`, `one-dark`, `solarized`); `applyTheme(name)` writes CSS vars onto `:root` and persists to `localStorage`

## Features

- **Search**: highlights matching keys/values across the tree, prev/next navigation
- **DeepPick filter**: a JSON-array schema in the filter pane calls `deepPick(target, schema)` to show a filtered subset of the tree
- **Path tracking**: `getPathSegs(rowEl)` walks DOM parents to build the path; `buildPathInto(container, segs)` renders it with colored segments
- **Location panel**: IDE-style sidebar rendered by `buildLocationTree` / `renderLocLevel`; shows hierarchy of the pinned path; togglable via header button
- **Tabs**: `addTab`, `closeTab`, `switchTab`, `renameTab`, `renderTabBar`; each tab stores its own input text
- **Keyboard shortcuts overlay**: toggle via `toggleShortcuts()`

## Running

```bash
./start.sh   # opens index.html in the default browser
```
