# JSON Viewer

A local, single-file JSON explorer. No server, no dependencies, no build step.

## Setup

```bash
git clone <repo>
cd json-viewer
open json-viewer.html   # or double-click it
```

Bookmark the local file URL in your browser. That's it.

`start.sh` does the same thing if you prefer.

## Data & persistence

Everything is stored in `localStorage` under the file's origin (the `file://` path or domain if served).

What's persisted across reloads:
- All tab contents (raw JSON input + tab names)
- Active tab
- Selected theme
- Location panel toggle state

**To clear everything:** Open DevTools → Application → Local Storage → delete the `json-viewer-*` keys. Or clear site data for the origin.

**How it breaks:**
- Open the file from a different path (e.g. renamed, moved, different machine) → different origin → blank slate, previous data unreachable
- Open in private/incognito → data is not written to persistent storage, lost on window close
- localStorage quota exceeded (~5MB) → saves silently fail, changes won't persist; clear old tabs if this happens
- Clear browser history/site data → all tabs gone
