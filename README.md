# phake DUMMY v0.03

A static, readable HTML/JavaScript prototype. No framework, build step, analytics, or backend.

## Files

- `index.html`: launch doorway
- `settings.html`: localStorage reader settings
- `style.css`: shared presentation
- `instructions.txt`: general odyssey instructions (plain text)
- `data/<slug>.txt`: per-odyssey material (plain text)

## Deployment

Upload the directory to GitHub Pages or another HTTPS static host. For a quick local test, run `python3 -m http.server 8000` in this directory and visit `http://localhost:8000/?slug=defenestration-cat`. Opening `index.html` directly with `file://` may fail because `fetch()` is restricted.

The two source-location constants are at the beginning of `index.html`'s JavaScript:

```javascript
const INSTRUCTIONS_URL = new URL("./instructions.txt", window.location.href);
const DATA_BASE_URL = new URL("./data/", window.location.href);
```

Change them to absolute HTTPS URLs if your sources are elsewhere. The browser must be able to fetch them (CORS applies for cross-origin sources), and ChatGPT must separately be able to retrieve them. A successful browser check does **not** prove ChatGPT can read the URLs.

Public odyssey URL example: `https://YOURHOST.example/?slug=defenestration-cat`.

Optional short override: `?slug=defenestration-cat&instructions=Focus+on+the+refrain` (up to 1,000 characters). The override is not a replacement for the instructions file.

The launch prompt includes the **URLs**, not the contents, of the two text files. The browser uses `fetch()` only to check existence and report sizes. `URLSearchParams` builds the ChatGPT `q` link; no gzip or custom character substitutions are used.

## Important limitations

- ChatGPT may be unable to retrieve public files or may read only part of them. The prompt explicitly requires disclosure of retrieval failures.
- A roughly 5,000-character launch-URL budget is a conservative experimental guard, not an official limit.
- Browser localStorage preferences are not parental controls. Use separate browser profiles on shared computers.
- Public text files are publicly inspectable; `robots.txt` is not secrecy or IP protection.
- Add the real song URL to `FIFTH_GRADE_SONG_URL` in `settings.html` when available.
- The browser's source-file check requires same-origin files or CORS-enabled cross-origin hosting.
- Changing settings while a doorway tab is already open requires reloading that doorway tab before launch.

## v0.03 — alternative instructions and direct-launch loading

- `?slug=defenestration_cat` displays the ordinary doorway.
- `?slug=defenestration_cat&g=1` displays a brief loading screen and redirects to ChatGPT as soon as the two source files pass checks.
- `?slug=defenestration_cat&i=instructions_1&g=1` selects `instructions_1.txt` in the GitHub repository root.
- If `i` is absent or empty, the default is `instructions.txt`. A valid but missing alternative file produces a visible error, **not** a silent fallback.
- `instructions=` still appends a short additional reader request and is independent of `i`.
- `g=1` uses `window.location.replace()`; it does not create an extra Back-history entry.
- On any validation/fetch error, the loading screen gives way to the regular doorway with the error message.
- The URL constants in `index.html` point to the user's GitHub Raw repository. Change `INSTRUCTIONS_BASE_URL` and `DATA_BASE_URL` if moving the files.
- The default `instructions.txt` must exist in the GitHub repository root. If your actual standard file is `instructions_1.txt`, change `DEFAULT_INSTRUCTIONS` to that filename.
- No artificial loading delay is introduced. Browser-local settings still apply in direct mode.
