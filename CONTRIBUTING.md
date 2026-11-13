# Contributing to ClackJoy

Thanks for helping. ClackJoy is intentionally a single self-contained HTML file, so most contributions are small, focused edits to `index.html` plus a note about how you tested them.

## Reporting bugs

Open an issue with:

- **Browser and version** (and OS).
- **Board** — model, and how it was loaded (WebHID, `.vil` + lookup, or manual `vial.json`/KLE).
- **Steps to reproduce** — what you clicked or typed, in order.
- **Expected vs. actual behaviour**, and a screenshot or console error if you have one.

If the problem involves a specific layout, attach the `.vil` file (strip anything private first).

## Adding a board definition

1. Find `var KNOWN_BOARDS` in `index.html`.
2. Add an entry with the display name, the board's path inside the vial-qmk repository, and its uid if the board reports one over WebHID:

   ```js
   { name: 'My Board', path: 'keyboards/vendor/myboard', uid: '123456789' }
   ```

   `UID_MAP` is derived from these entries automatically; don't edit it by hand.
3. Bring your own board's `vial.json` from its firmware repository (usually `keyboards/<vendor>/<board>/keyboards/vial.json` or `keymaps/vial/vial.json`) and confirm ClackJoy renders it correctly before opening a PR.

If the uid is unknown, add the entry without `uid` and say so in the PR — the board will still be loadable by path and via manual upload.

## Adding word lists or texts

Add content to the `WORDS` list or the `TEXTS` array in `index.html`.

**Licensing rule: contributed word lists and practice texts must be public domain or your own original work.** Do not paste copyrighted articles, books, lyrics, or subtitled dialogue. If a text is public domain, state the source and why it is public domain in the PR. If you wrote it, say so. Maintainers may remove content whose provenance is unclear.

## Adding a UI language

Host-layout character tables live in `var LANG_MAPS` (search for `name: 'US QWERTY'`), and the selectable list is validated against `['us','uk','de','fr','pl']` in the settings sanitiser. To add a language:

1. Add a table with the layout's unshifted/shifted characters for each key.
2. Add it to the validation list and to the language selector in the settings panel.
3. Check that lessons, key labels, and the next-key preview all use the new table.

## Coding conventions

- **Single file.** Keep HTML, CSS, and JavaScript in `index.html`. No dependencies, no CDN links, no build step.
- **Section markers.** Preserve the existing `/* ---------------- ... ---------------- */` banners and keep new code inside the section that owns the behaviour.
- **Colours via CSS variables.** Define colours as CSS custom properties (`--text`, `--line`, `--accent`, …) and use them in rules; don't hard-code new hex colours in styles or inline styles. Theme switching depends on this.
- **Offline first.** Never add a runtime dependency on a third-party service. The app must work with `file://` and with no network.
- **Defensive parsing.** Board definitions and `.vil` files are untrusted input; validate shape and size, and fail with a clear message rather than throwing into the console.
- **No tracking.** No analytics, no telemetry, no remote logging.

## Testing

There is currently no automated test suite in this repository — the `tests/` directory does not exist yet, and testing is ad hoc. **A `tests/` Node harness would be a welcome first contribution.** If you add one, document the command here (for example `node --test tests/`) and keep it dependency-free so it runs on a clean checkout.

Until then, verify changes by running the browser checklist below and describe the result in your PR:

- **Typing** — a lesson, words, timed, texts, n-grams, a layer drill, weak keys, and custom text all start and register keystrokes.
- **Layers** — layer tabs switch, pinning works, and the next-key preview follows the active/pinned layer.
- **Themes** — light, dark, and system all render correctly and persist across reloads.
- **Sounds** — enable sound, preview each error and correct sound, and hear the selected ones during a run.
- **`.vil` import** — import a `.vil` for a known board (definition lookup) and for an unknown board (manual `vial.json`), and confirm the geometry and keymap are correct.
- **Stats** — finish a run, open the dashboard, and check that history, per-finger stats, and export/reset behave.
- **Offline** — open `index.html` directly from disk with the network disabled (apart from a known-board definition fetch, which is expected to need network the first time).

## License

By contributing, you agree that your contributions are licensed under the GNU General Public License v3.0, the same license as the project.
