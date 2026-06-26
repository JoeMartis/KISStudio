# KISS Production Guide — From Authoring to a Live Module

This guide is for the **production team**. It explains how to take the two things
KISS produces and turn them into **Content Libraries** and a finished
**module** in Open edX.

> For *authoring* in KISS itself, see the **[Author Guide](AUTHOR_GUIDE.md)**.
> This guide picks up where that one ends — at the exported files.

---

## 1. The big picture

KISS (Keep It Simple Studio) is the **planning + authoring** front end. It does **not** talk to your
platform (no API). Instead it hands production two artifacts, and you do the build
in the Studio UI:

```
   KISS  ──exports──▶  ┌─ OLX package (.tar.gz)   → the assembled module
                            └─ Library worksheet (CSV) → the plan for the library
                                         │
                       (+ assets: images, video sources/OVS IDs, transcripts)
                                         │
                                         ▼
                    Open edX:  Content Library (reusable, tagged blocks)
                                         │
                                         ▼
                               Module / Course (structure + content)
                                         │
                                         ▼
                                   QA  →  Publish
```

### The model (how KISS's fields map to your platform)

| KISS                              | Becomes in production                                  |
|----------------------------------------|--------------------------------------------------------|
| **ORG**                                | One **Content Library** (per program/org)              |
| **Display name** (module title)        | A **Collection** inside that library (this module)      |
| **Section** (Introduction / Lectures / Recitations / Assignments / Conclusion) | **Work segment** tag, and a course **section**          |
| **Concept — Type** (component name)    | The **block's name** in the library and in Studio       |
| **Modality** (Text / Video / Problem)  | A tag, and the block type you create                    |
| **Type** (Lecture / KC / Problem / …)  | A tag                                                   |
| **Video filename**                     | A production note (which source to attach to a video)   |
| **Status / Owning team / Clearance / LD tags** | Tags you fill in as the team processes each block |

---

## 2. The two outputs, and what each is for

### A. The OLX package (`<course>_<run>.tar.gz`)
Produced by **`#olx` → Export OLX**. It's a complete, importable course:

- the full structure (sections → subsections → units),
- every **text** block as HTML,
- every **problem** as a working edX problem (including the inline/table
  multi-dropdowns),
- every **video** as a **named placeholder** (`<video display_name="…"/>` — the
  source is attached later, see §6),
- a grading policy where **Assignments** count toward the grade.

**Use it to stand up the module fast** — one import gives you the whole skeleton
with all the text and problems already in place and correctly named.

### B. The Library worksheet (CSV + on-screen table)
Produced by **`#olx` → Library worksheet → Download CSV**. One row per component,
with the columns you'll work through:

`Library · Collection · Work segment · Type · Modality · Suggested name · Concept ·
Video file · Status · Owning team · Clearance · LD tags · Location · In library? · Notes`

**Use it as the build plan and the tracker** for populating and tagging the
library — it tells you exactly what blocks to create, what to name them, where each
lives, and (the empty columns) what to tag.

---

## 3. Before you start — handoff checklist

Get these from the author before production begins:

- [ ] **The module is "green" in KISS** — the issues panel shows *✓ No issues*.
- [ ] **OLX package** exported (`#olx` → Export OLX).
- [ ] **Library worksheet CSV** exported (`#olx` → Library worksheet → Download CSV).
- [ ] **Images** — every file referenced with `[[image: …]]` in the content,
      with the exact filenames.
- [ ] **Video sources** — the OVS source for each video (the worksheet's
      **Video file** column tells you which file goes with which block).
- [ ] **Transcripts** — the `.srt`/caption files for each video.
- [ ] **Defaults decided** — owning team, clearance, status, and any LD tags your
      team applies (so tagging is consistent).

> **Tip:** open the worksheet CSV in Excel/Sheets and fill the **Owning team**,
> **Clearance**, and **LD tags** columns up front with your team's defaults, so
> Phase 4 tagging is copy-down rather than think-each-time.

---

## 4. Choose your build order

There are two valid orders. Pick based on your goal:

| Goal | Order | Why |
|------|-------|-----|
| **Reuse-first** (blocks shared across many modules) | **Library → Module** (§5 then §7) | The library is the source of truth; the course references its blocks. |
| **Speed-first** (get this module standing quickly) | **Module → Library** (§7 then §5) | Import the OLX to get a working module now; harvest blocks into the library afterward. |

Either way you do the same work — the difference is just which artifact you build
first. The sections below are written **reuse-first** (the order most teams want);
if you're speed-first, do §7 first and treat §5 as "seed the library from the
imported components."

---

## 5. Build the Library (from the worksheet)

**Goal:** a tagged, reusable block in the library for every worksheet row.

1. **Create the library** — one per program/org. Name it from the worksheet's
   **Library** column (your ORG).
2. **Create the Collection** for this module — name it from the **Collection**
   column (the module's display name). Every block for this module goes in here.
3. **Work down the worksheet, one row = one block.** For each row:
   - Create a block of the row's **Modality**: Text/HTML → an HTML block; Problem
     (CAPA) → the matching problem type; Video → a video block.
   - **Name it exactly the "Suggested name"** (`Concept — Type [qualifier]`). This
     keeps names consistent and searchable — the **Concept** is the word a builder
     would search for.
   - **Fill in the content.** Fastest source is the OLX package: import it to a
     scratch/build course (§7) and copy each component's body across; or use
     KISS's on-screen **preview** as the reference. (Problems and text come
     across verbatim; videos are placeholders you finish in §6.)
   - **Tag the block**: **Work segment**, **Modality**, **Type**, **Status**,
     **Owning team**, **Clearance**, and any **LD tags** — straight from the
     worksheet row.
   - **Mark it done** — set **In library? = yes** (or **Status = Done**) on that
     worksheet row so the team can see progress at a glance.
4. **Upload images** referenced by `[[image: …]]` to the library/course assets,
   using the **exact filenames**.

> The worksheet's on-screen table groups rows by **Work segment** and flags any
> block still **needing a Concept** — clear those flags before tagging, since the
> Concept is the block's name.

---

## 6. Finish the media (videos, transcripts, images)

KISS intentionally leaves video **sources** and **transcripts** out of the
export — they're attached here, in production:

- **Video source** — for each video block, attach its OVS source. The worksheet's
  **Video file** column tells you which file belongs to which block (match by the
  block's name / Location).
- **Transcript** — upload the caption file to the video block.
- **Images** — confirm every `[[image: …]]` filename has been uploaded to assets,
  so the references resolve.

Do this once per video/image; if you built the library first (§5), attach to the
library block so every module reusing it inherits the finished media.

---

## 7. Assemble the Module (course)

**Goal:** the course structure with the right content in every unit.

**Fast path — import the OLX:**
1. In **Studio → Tools → Import**, upload the `<course>_<run>.tar.gz`.
2. You now have the entire structure (sections → subsections → units) with all
   **text** and **problems** in place and named, plus **video placeholders**.
3. Finish the media (§6) and confirm images are uploaded.
4. Grading is already set so **Assignments** count toward the grade — confirm it
   matches your policy.

**Composed path — build from library blocks:**
1. Recreate the structure: course **sections** = KISS sections (Introduction /
   Lectures / Recitations / Assignments / Conclusion), then subsections and units,
   following the worksheet's **Location** column (`Section > Subsection > Unit`).
2. Into each unit, add the library blocks **by reference** (so updates to a library
   block flow into the course).
3. Mark **Assignments** subsections as graded.

> **Recitations is optional.** If the author removed it, it simply won't be in the
> OLX or the worksheet — don't add it.

---

## 8. QA & publish

Run this pass before publishing — the worksheet is your checklist:

- [ ] **Structure** matches the KISS outline (sections, subsections, units in
      order). Use KISS's **Outline** (with the type filters) as the reference.
- [ ] **Every worksheet row** has a block, named correctly, tagged, **In library? = yes**.
- [ ] **Problems** render and grade correctly — spot-check the **inline/table
      dropdowns** especially (each dropdown gradeable; correct answers right).
- [ ] **Graded vs. ungraded** is right — Assignments graded, the rest knowledge checks.
- [ ] **Videos** play (source attached) and have **transcripts**.
- [ ] **Images** load (filenames match).
- [ ] **Names** read cleanly as standalone library blocks (`Concept — Type`).
- [ ] **Publish** the units/subsections, then the course.

---

## 9. Roles & tracking

The worksheet is the **single source of truth** for who's doing what and how far
along each block is:

- **Owning team** — who builds/owns each block.
- **Status** — drive each row `Not started → In progress → Done`.
- **In library?** — flips to *yes* when the block exists and is tagged.
- **Notes** — anything production needs to flag (missing asset, rework, etc.).

Re-export the worksheet from KISS any time the author changes the module; the
rows regenerate from the current content (your filled-in tag columns are your own
copy, so keep your working CSV as you go).

---

## Appendix — quick reference

**Worksheet columns:** Library · Collection · Work segment · Type · Modality ·
Suggested name · Concept · Video file · Status · Owning team · Clearance · LD tags ·
Location · In library? · Notes

**Sections (work segments):** Introduction · Lectures · Recitations *(optional)* ·
Assignments *(graded)* · Conclusion

**Type vocabulary:** Lecture · KC · Problem · Worked example · Reading · Diagram ·
Recitation · Discussion

**Modality:** Text / HTML · Video · Problem (CAPA)

**Naming convention:** `Concept — Type [optional qualifier]` (e.g.
*"Reading a forecast — KC"*). The **Concept** is the searchable idea and becomes the
block's library name.

**Attached in production (not in the export):** video sources (OVS), transcripts.
**Referenced by filename (upload to assets):** images.
