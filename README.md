# JSON Viewer

A local, single-file JSON explorer. No server, no dependencies, no build step.

## Use it online

**[json-tool.kristerkasemaa.com](http://json-tool.kristerkasemaa.com/)** — no install needed.

## Self-host / run locally

```bash
git clone git@github.com:KristerKasemaa/json-viewer.git
cd json-viewer
open json-viewer.html   # or double-click it
```

Bookmark the local file URL in your browser. That's it.

`start.sh` does the same thing if you prefer.

## Privacy & analytics

All JSON data you paste stays in your browser. It is never sent to any server.

The hosted version at [json-tool.kristerkasemaa.com](http://json-tool.kristerkasemaa.com/) includes [GoatCounter](https://www.goatcounter.com/), a privacy-friendly, open-source analytics service. It collects **only** basic visit counts (pageviews, unique visitors, referrer, browser/OS). It uses no cookies and stores no personal data.

**Verify it yourself:** Open DevTools (F12) → Network tab → reload the page. The only external request is a single call to `gc.zgo.at/count.js` and a hit to `krister.goatcounter.com/count`. No request ever contains anything you typed into the app.

Running locally via `file://` — GoatCounter does not load (it requires an HTTP origin), so there is zero tracking.

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
