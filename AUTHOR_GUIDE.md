# UL Module Builder — Author Guide

This guide explains how to write a complete course module in **UL Module Builder**
and turn it into something you can import into edX Studio. It's written for
content authors — no coding required.

**Contents**

1. [How the tool works](#1-how-the-tool-works)
2. [The module structure](#2-the-module-structure)
3. [Adding components](#3-adding-components)
4. [Naming: Concept, Type, and Qualifier](#4-naming-concept-type-and-qualifier)
5. [Formatting text (the markdown you can use)](#5-formatting-text-the-markdown-you-can-use)
6. [The six problem types](#6-the-six-problem-types)
7. [Inline dropdowns (in a sentence and in tables)](#7-inline-dropdowns-in-a-sentence-and-in-tables)
8. [Videos](#8-videos)
9. [Saving, resuming, and not losing work](#9-saving-resuming-and-not-losing-work)
10. [The issues panel (validation)](#10-the-issues-panel-validation)
11. [Reordering, the outline, and the preview](#11-reordering-the-outline-and-the-preview)
12. [Exporting to edX (OLX)](#12-exporting-to-edx-olx)
13. [The Library worksheet (naming & tagging)](#13-the-library-worksheet-naming--tagging)
14. [Tips, limits, and gotchas](#14-tips-limits-and-gotchas)
15. [Appendix: the `.txt` file format](#appendix-the-txt-file-format)

---

## 1. How the tool works

UL Module Builder is a single web page. You type your module into it, and it keeps
everything in the browser as you work. When you're done (or want to stop), you
**export** your work to a plain-text `.txt` file. To continue later, you
**import** that file.

There are three outputs you can produce:

- **`.txt` file** — your editable source of truth. Save it, share it, re-open it.
- **OLX `.tar.gz`** — the package you import into edX Studio (hidden behind
  `#olx`; see [§12](#12-exporting-to-edx-olx)).
- **Library worksheet CSV** — a naming/tagging checklist for Content Libraries
  (also behind `#olx`; see [§13](#13-the-library-worksheet-naming--tagging)).

> 🔑 **The golden rule:** your work is **not** saved automatically. Export your
> `.txt` before you close or refresh the tab. See [§9](#9-saving-resuming-and-not-losing-work).

---

## 2. The module structure

Every module has the **same fixed five sections** (chapters), in this order:

| Section          | What goes here                                            |
|------------------|-----------------------------------------------------------|
| **Introduction** | Module overview and learning goals                        |
| **Lectures**     | The core teaching content, organized into lectures        |
| **Recitations**  | Walkthroughs, tooling, worked sessions                    |
| **Assignments**  | Graded work (problems here count toward the grade)        |
| **Conclusion**   | Wrap-up and next steps                                     |

You can't rename, remove, or reorder these five sections — they're the backbone of
every module. Inside them, the hierarchy is:

```
Section (chapter)
└─ Subsection (a lecture, recitation, or assignment)
   └─ Unit (a single page the learner sees)
      └─ Component (a block of text, a video, or a problem)
```

- **Subsections** are things like "Lecture 1: Foundations." You can add, rename,
  reorder, and delete them (except the fixed scaffold ones).
- **Units** are individual pages. Each lecture typically opens with an
  *Overview and Learning Objectives* unit and closes with a *Summary* unit — these
  are created for you and can't be deleted (you can edit their content).
- **Components** are the actual content blocks. A unit can hold several.

> **Why "Assignments" is special:** problems placed under **Assignments** are
> exported as **graded**. Problems anywhere else are treated as ungraded knowledge
> checks. (See how this affects naming in [§4](#4-naming-concept-type-and-qualifier).)

---

## 3. Adding components

Inside a unit, use the **+ Add** controls to insert a component. There are three
kinds:

| Component   | Use it for                                              |
|-------------|---------------------------------------------------------|
| **TEXT**    | Readings, explanations, summaries, diagrams (with image) |
| **VIDEO**   | A video (enter its OVS ID)                                |
| **PROBLEM** | Any of the six interactive question types ([§6](#6-the-six-problem-types)) |

Each component has a few fields. The most important content fields:

- **TEXT** → a **Title** (optional) and a **Body** (your formatted text).
- **VIDEO** → a **Title**, the **Video** field (the OVS ID), and an
  optional **Transcript file** name.
- **PROBLEM** → a **Problem type**, a **Question**, type-specific answer fields,
  and optional **Explanation** and **Hint**.

Every component also has the naming fields **Concept**, **Type**, and **Qualifier**
— covered next.

---

## 4. Naming: Concept, Type, and Qualifier

Each component gets an automatic, human-readable name used in the edX export and in
the Library worksheet. The pattern is:

```
<Concept> — <Type> [optional qualifier]
```

For example, a knowledge-check problem about reading a forecast becomes:

```
Reading a forecast — KC
```

### Concept

A short phrase naming **the idea this component teaches or tests** — e.g.
*"Features and labels"*, *"Model evaluation metrics"*, *"Annual return"*. You type
this in the **Concept** field. If you leave it blank, the tool falls back to the
component's title or its unit's title (and the worksheet flags it as needing a
concept).

### Type

A label for **what kind of object** this is. It's chosen from a fixed list, and the
tool picks a sensible default you can override:

| Type            | Default for…                                  |
|-----------------|-----------------------------------------------|
| **Lecture**     | Videos                                         |
| **Reading**     | Text components                                |
| **KC**          | Problems (knowledge check) outside Assignments |
| **Problem**     | Problems inside **Assignments** (graded)       |
| **Worked example** | (choose manually)                           |
| **Diagram**     | (choose manually — e.g. a text block that's mainly an image) |
| **Recitation**  | (choose manually)                              |
| **Discussion**  | (choose manually)                              |

The dropdown shows which option is the default; pick another if it fits better.

### Qualifier

An optional parenthetical that distinguishes near-identical components — e.g.
`(select all)` for a checkboxes problem, or `(part 2)`. It appears in parentheses
at the end of the name.

---

## 5. Formatting text (the markdown you can use)

Wherever you write prose — **Body**, **Question**, **Explanation**, **Hint** — you
can use a small set of formatting marks. The live preview shows how it will look.

| You type | You get |
|---|---|
| `*bold*` | **bold** |
| `_italic_` | *italic* |
| `[link text](https://example.com)` | a hyperlink |
| `[[image: filename.png \| alt text]]` | an image (see note below) |
| `- item` (one per line) | a bulleted list |
| `1. item` (one per line) | a numbered list |
| a blank line | a paragraph break |

### Tables

Write a table using pipes `|`, with a separator row of dashes under the header:

```
| Model               | Verdict   |
| ---                 | ---       |
| Coin-flip predictor | no better |
| Tuned gradient model| beats it  |
```

The first row is the header; the `| --- | --- |` line marks the boundary; the rest
are data rows.

**Need a literal `|` inside a cell?** (For example, conditional-probability
notation.) Escape it with a backslash: write `P(A\|B)` to get a single cell reading
`P(A|B)`.

### Images and transcripts are *referenced*, not uploaded

When you use `[[image: confusion_matrix.png | …]]` or set a video transcript file,
the tool writes a **reference** to that filename. The image/transcript itself is
**not** bundled into the export. After importing into edX, upload those assets
separately (**Files & Uploads** in Studio) using the **same filenames**.

---

## 6. The six problem types

Choose a type from the **Problem type** dropdown. Each shows the right fields.

### Multiple choice (`multiple_choice`)
One correct answer from a list. Mark exactly one option correct (tap the circle
marker). Learners pick one.

### Checkboxes (`checkboxes`)
"Select all that apply." Mark **one or more** options correct. Tip: add the
qualifier `(select all)` so it's obvious in the name.

### Dropdown (`dropdown`)
A single dropdown menu with one correct choice. (For *several* dropdowns in one
problem, use **inline dropdowns** — [§7](#7-inline-dropdowns-in-a-sentence-and-in-tables).)

### Inline dropdowns (`inline_dropdown`)
Multiple dropdowns embedded in a sentence and/or a table. Covered in detail in
[§7](#7-inline-dropdowns-in-a-sentence-and-in-tables).

### Numerical (`numerical`)
The learner types a number. You provide the **Answer** and an optional
**Tolerance** (how far off is still correct, e.g. `0.5`). edX accepts plain numbers
**and** math expressions as the answer — e.g. `12`, `-3.5`, `1/2`, `pi`, `sqrt(2)`.

### Short text (`text_input`)
The learner types a short text answer, matched case-insensitively against the
**Answer** you provide. Good for one-word or short-phrase responses.

**For every problem you can also add:**
- an **Explanation** — shown after answering, supports full formatting;
- a **Hint** — shown on request, supports formatting.

> Each option's text and the question prose all support the same formatting from
> [§5](#5-formatting-text-the-markdown-you-can-use).

---

## 7. Inline dropdowns (in a sentence and in tables)

The **inline dropdowns** type lets one problem contain **several** dropdown menus —
either embedded mid-sentence or placed in the cells of a table. This is ideal for
"classify each of these" or "fill in the blanks" exercises.

### How it works

1. Set **Problem type** to **inline dropdowns**.
2. In the **Body**, write your prose and put a **token** — `[[1]]`, `[[2]]`,
   `[[3]]`, … — everywhere you want a dropdown. Tokens can sit in a sentence **or
   inside a table cell**.
3. Below the body, you'll see a **dropdown editor** for each token. Fill in each
   dropdown's options and mark the one correct choice.

The **+ Insert dropdown** button adds the next dropdown *and* drops its `[[n]]`
token at your cursor — the easiest way to add one.

### Example

Body:

```
A forecast that always predicts last year's value is a [[1]] baseline.

Classify each model by how it compares to that baseline:

| Model                | Verdict |
| ---                  | ---     |
| Coin-flip predictor  | [[2]]   |
| Tuned gradient model | [[3]]   |
```

Then three dropdown editors appear — one each for `[[1]]`, `[[2]]`, `[[3]]` — where
you set the options and correct answer. On export, this becomes a single edX
problem with three independently-graded dropdowns: one inline, two in the table.

### Good to know

- **Each token needs a matching dropdown, and vice-versa.** The issues panel warns
  you about an orphan `[[3]]` with no dropdown, or a dropdown with no token.
- **Removing a dropdown renumbers the tokens for you** so they stay in order.
- **Don't reuse the same number** (`[[1]]` twice) unless you really mean two
  separate dropdowns with the same options — the tool warns if you do.
- **Math in the *choices* is plain text.** The dropdown menus are native edX
  selects, which are text-only. Fractions/symbols go in as text or unicode
  (`½`, `1/2`, `WT/2`). Math in the *question prose above* the dropdowns renders
  normally.

---

## 8. Videos

In a **VIDEO** component, enter the video's **OVS ID** in the **Video** field (OVS
is our video hosting service).

Optionally add a **Transcript file** name (e.g. `L1-1.srt`). As with images, the
transcript file is referenced by name — upload it to Studio separately.

---

## 9. Saving, resuming, and not losing work

**Your work lives only in the browser tab until you export it.** There is no
auto-save and no cloud. If you refresh or close the tab, unsaved work is lost (the
tool warns you when you try to leave with unsaved changes).

- **Export `.txt`** — downloads your module as an editable text file. Do this
  often. Keep it somewhere safe (shared drive, repo, etc.).
- **Import `.txt`** — loads a previously exported file so you can keep editing.

The `.txt` file is the canonical, human-readable source for your module. It's also
safe to hand-edit carefully (see the [appendix](#appendix-the-txt-file-format)) —
but **keep the five section headings exactly as they are**. If you rename a section
heading or duplicate one, the importer can't place that content and will **skip
it**, telling you which section it dropped. (It won't silently lose your work, but
it also won't import a mislabeled section.)

---

## 10. The issues panel (validation)

The tool continuously checks your module and lists problems to fix before handoff.
Common messages and what they mean:

- **Missing display name / course ID** — fill in the module details.
- **Unit has no components** — add at least one component, or remove the unit.
- **Empty text component** — the text block has no title and no body.
- **Video has no ID or URL** — the Video field is empty.
- **Problem has no question / no answer** — fill the missing field.
- **Choice problem needs exactly one correct answer** — mark exactly one (for
  multiple choice / dropdown).
- **Checkboxes: no answer marked correct** — mark at least one.
- **Choice problem needs at least two options** — add more options.
- **Numerical answer isn't a number** — the answer field has no number/expression.
- **Inline dropdowns** warnings — orphan `[[n]]` tokens, dropdowns missing a token,
  fewer than two options, no correct option, blank options, or a repeated token.

A clean module shows **"✓ No issues — ready to download."**

---

## 11. Reordering, the outline, and the preview

- **Reorder** subsections, units, components, and options by **dragging the ⠿
  handle**, or by focusing it and pressing **↑ / ↓**. Fixed scaffold items
  (Overviews, Summaries, the five sections) stay put.
- **Outline** — a clickable table of contents. It updates live as you add and
  reorder things, and includes videos and problems, so you can jump anywhere.
- **Preview** — a readable rendering of your module. It's hidden until you reveal
  it (to keep the workspace focused) and updates as you type.
- **Light / dark mode** — toggle with the theme button; your choice is remembered.

---

## 12. Exporting to edX (OLX)

The edX export is intentionally kept out of the way so only those who need it use
it.

### Steps

1. In the app's URL, add **`#olx`** at the end and press Enter
   (e.g. `https://…/index.html#olx`). Two new buttons appear.
2. Click **Export OLX**. The tool builds an Open edX **OLX** package and downloads
   it as **`<course-id>_<run>.tar.gz`**.
3. In **edX Studio**, go to **Tools → Import** (or **Settings → Import**) and
   upload that `.tar.gz`.
4. After import, **upload your assets** — every image referenced with
   `[[image: …]]` and every transcript file — under **Files & Uploads**, using the
   exact filenames.

### What's in the package

The export contains the full course structure (sections → subsections → units →
components), with:

- text components as HTML pages,
- videos as edX video components (with transcripts referenced),
- problems as edX CAPA problems (including the multi-dropdown ones),
- a basic grading policy where **Assignments** count toward the grade.

> **Before you import for real,** it's worth doing one test import into a sandbox
> course to confirm everything lands the way you expect — especially the first time
> you use a new problem type.

---

## 13. The Library worksheet (naming & tagging)

Also revealed by **`#olx`**, the **Library worksheet** helps you move components
into Open edX **Content Libraries** with consistent names and tags. It gives you
both an **on-screen table** (eyeball it, copy it) and a **CSV download**.

It follows a simple model:

- **One library per program/org** — taken from your **ORG** field.
- **One collection per module** — taken from your **DISPLAY_NAME** (the module
  becomes a collection inside the library).
- **Work segment** = the section the component lives in (Introduction, Lectures, …).
- **Modality** = Text / HTML, Video, or Problem (CAPA).
- **Type** = the convention Type from [§4](#4-naming-concept-type-and-qualifier).
- **Suggested name** = the `Concept — Type [qualifier]` name.

The CSV opens cleanly in Excel/Google Sheets (it includes a UTF-8 marker so accents
and the em-dash render correctly, and is safe against spreadsheet formula quirks).
It has columns for owning team, status, clearance, and LD tags that you fill in as
your team processes each component.

> The on-screen table flags any component that still **needs a Concept** so you can
> spot gaps before tagging.

---

## 14. Tips, limits, and gotchas

- **Export before you leave.** (Said three times for a reason.) No auto-save.
- **Keep the five section headings intact** in any hand-edited `.txt`. Renamed or
  duplicated headings get skipped on import (you'll be told which).
- **Assets aren't bundled.** Images and transcripts are referenced by filename;
  upload them to Studio separately with matching names.
- **Math in dropdown choices is text-only.** Use unicode/text for fractions and
  symbols in dropdown options. Prose math (in the question) renders fine.
- **Numerical answers can be expressions.** `1/2`, `pi`, `sqrt(2)` are all valid —
  edX evaluates them.
- **Graded vs. ungraded** is decided by section: problems in **Assignments** are
  graded; elsewhere they're knowledge checks.
- **Videos use your OVS ID** — enter it in the Video field.
- **Do a sandbox test import** the first time, to confirm the result in Studio.

---

## Appendix: the `.txt` file format

You normally never need this — the app reads and writes the file for you. But if
you or a teammate want to hand-edit, here's how the format works. **Edit
carefully** and keep the structure intact.

### Overall shape

```
================================================================================
MODULE
================================================================================
DISPLAY_NAME: AI and Finance
COURSE_ID: UAI.FIN.1
ORG: UAI_SOURCE
RUN: 1T2026
SHORT_DESCRIPTION: A hands-on introduction to …

================================================================================
CHAPTER: Introduction
================================================================================

--------------------------------------------------------------------------------
SEQUENTIAL: Module Overview and Learning Goals
--------------------------------------------------------------------------------

>>> UNIT: Module Overview and Learning Goals
>>> COMPONENT: TEXT
CONCEPT: Module overview
QUALIFIER:
LIB_TYPE:
TITLE: Welcome to AI and Finance
BODY:
Welcome to *AI and Finance*. …
<<<
```

### The rules

- **Section headers** use the `====` banner with `CHAPTER: <name>`. The five names
  (Introduction, Lectures, Recitations, Assignments, Conclusion) are fixed — don't
  change them.
- **Subsections** use the `----` banner with `SEQUENTIAL: <title>`.
- **Units** start with `>>> UNIT: <title>`.
- **Components** start with `>>> COMPONENT: TEXT` / `VIDEO` / `PROBLEM`.
- **Single-line fields** are `KEY: value` — e.g. `TITLE:`, `CONCEPT:`,
  `QUALIFIER:`, `LIB_TYPE:`, `VIDEO_URL:`, `TRANSCRIPT_FILE:`, `PROBLEM_TYPE:`,
  `ANSWER:`, `TOLERANCE:`, `HINT:`.
- **Multi-line fields** (`BODY:`, `QUESTION:`, `EXPLANATION:`) start with the key
  on its own line, then your content, then a closing `<<<` fence. Everything
  between the key line and the `<<<` is kept exactly as written.

### Problem options

For multiple choice / dropdown (single answer), use round markers; `(x)` marks the
correct one:

```
OPTIONS:
- ( ) The value the model predicts
- (x) An input variable the model uses
- ( ) The model's accuracy
```

For checkboxes (select all), use square markers; `[x]` marks each correct one:

```
OPTIONS:
- [x] Precision
- [x] Recall
- [ ] Learning rate
- [x] F1 score
```

### Inline-dropdown blocks

The body uses `[[1]]`, `[[2]]`, … tokens; each dropdown is a `DROPDOWN n:` block
with option lines (round markers, `(x)` = correct):

```
PROBLEM_TYPE: inline_dropdown
QUESTION:
A forecast that always predicts last year's value is a [[1]] baseline.

| Model                | Verdict |
| ---                  | ---     |
| Coin-flip predictor  | [[2]]   |
| Tuned gradient model | [[3]]   |
<<<
DROPDOWN 1:
- (x) naive
- ( ) sophisticated
DROPDOWN 2:
- ( ) beats baseline
- (x) no better than baseline
DROPDOWN 3:
- (x) beats baseline
- ( ) no better than baseline
EXPLANATION:
The naive baseline is the bar to beat …
<<<
HINT: Compare each to *predicting last year again*.
```

### Escaping inside multi-line blocks

If a line of body/question content needs to literally start with `<<<`, `>>>`, or a
backslash, the tool prefixes it with a backslash when saving and removes it when
loading — so your content round-trips exactly. You generally don't need to think
about this; just know that the app preserves your text faithfully.

---

*Questions or something not behaving as described? The in-app self-test
(`#selftest` in the URL) verifies the core behaviors, and `sample_module.txt` is a
working reference you can import and dissect.*
