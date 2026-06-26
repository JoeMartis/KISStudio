# KISStudio

**Keep It Simple Studio** — a single-file web app for authoring structured course modules and exporting them
to **Open edX / edX Studio** (OLX) — no build step, no server, no install. Open
`index.html` in a browser and start writing.

> **New to authoring with this tool?** Read the **[Author Guide](AUTHOR_GUIDE.md)** —
> it walks through every field, formatting option, and problem type with examples.

---

## What it does

- **Author a whole module** — text, videos, and problems — in a guided, fixed
  structure (Introduction → Lectures → Recitations → Assignments → Conclusion).
- **Six problem types**: multiple choice, checkboxes (select-all), dropdown,
  **inline dropdowns** (multiple selects in a sentence *and* in tables),
  numerical, and short text.
- **Lightweight formatting** — bold, italic, links, images, lists, and tables —
  with a live preview.
- **Save / resume** by exporting and re-importing a plain-text `.txt` file.
- **Export to edX** — produce an importable `.tar.gz` (OLX) for edX Studio.
- **Library worksheet** — a naming-and-tagging checklist (CSV + on-screen table)
  for moving components into Content Libraries.

Everything runs locally in your browser. Nothing is uploaded anywhere.

---

## Quick start

1. **Open the app.** Either:
   - Visit the hosted GitHub Pages URL, or
   - Download `index.html` and open it in Chrome, Edge, Firefox, or Safari.
2. **Fill in the module details** (display name, course ID, etc.).
3. **Add content** under each section using **+ Add** buttons.
4. **Export your work** (the **Export `.txt`** button) before closing the tab —
   your work lives only in the browser until you do.
5. To pick up later, use **Import `.txt`** and choose the file you saved.

A ready-made example, `sample_module.txt`, is included — import it to see a fully
built module covering every component and problem type.

> ⚠️ **Your work is not auto-saved.** If you refresh or close the tab without
> exporting, it's gone. Export early, export often.

---

## Exporting to edX (OLX)

The edX export is a power-user feature, kept lightly hidden:

1. Add `#olx` to the end of the app's URL and press Enter
   (e.g. `…/index.html#olx`). Two extra buttons appear.
2. Click **Export OLX** to download `<course>_<run>.tar.gz`.
3. In **edX Studio → Tools → Import**, upload that file.

The same `#olx` view exposes the **Library worksheet** (naming + tagging
checklist). See the [Author Guide](AUTHOR_GUIDE.md#12-exporting-to-edx-olx) for the
full walkthrough, including how image assets are handled.

---

## Hidden URL features

| URL suffix   | What it reveals                                              |
|--------------|-------------------------------------------------------------|
| `#olx`       | **Export OLX** + **Library worksheet** buttons              |
| `#selftest`  | Runs the in-page self-test suite and shows a pass/fail panel |

---

## Repository contents

| File                | Purpose                                              |
|---------------------|------------------------------------------------------|
| `index.html`        | The entire application (HTML + CSS + JS, single file) |
| `sample_module.txt` | A complete example module — import it to explore      |
| `AUTHOR_GUIDE.md`   | Full authoring documentation for the content team     |
| `PRODUCTION_GUIDE.md` | How production turns the exports into libraries + a module |
| `README.md`         | This file                                             |

---

## Hosting

It's a static file. To host on **GitHub Pages**: enable Pages for the repository
(Settings → Pages → deploy from the default branch); `index.html` at the root is
served automatically. Any static host (or just opening the file locally) works
too.

---

## Browser support

Works in current versions of Chrome, Edge, Firefox, and Safari. The OLX `.tar.gz`
export uses the browser's built-in gzip (`CompressionStream`), available in
Safari 16.4+ and all recent Chromium/Firefox releases. If a browser can't gzip,
the tool falls back to a plain `.tar` you can compress yourself.

---

## For developers

`index.html` is self-contained — open it and edit. There is no build step.

- **In-app self-test:** append `#selftest` to the URL to run the built-in suite.
- The authoring file format and the OLX mapping are documented in the
  [Author Guide](AUTHOR_GUIDE.md#appendix-the-txt-file-format).
