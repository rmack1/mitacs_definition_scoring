# Meaning Test Coder

A single-file, no-backend scoring tool for the pre/intermediate/post meaning tests,
matching the same style as the Spelling & Story Coder. Everything runs client-side in the
browser — data is stored in `localStorage`, and you export CSV/JSON when you're ready.

This is a **separate site** from the Spelling & Story Coder — set it up as its own GitHub
repo (or its own folder within a repo, with its own GitHub Pages URL), since it uses
different data and its own browser storage.

## Test phase selector

After loading a participant, you'll see three buttons — **Pre-Meaning Test**,
**Intermediate Test**, and **Post-Meaning Test**. Only the section for the phase you pick
is shown, so a scorer working on one phase isn't distracted by the others. Data for every
phase is still saved together under the same participant record regardless of which one is
currently displayed.

## Your rubric is built in

The scoring rubric you provided (definitions and examples for scores 0–3, for all 12 words)
is now built into the tool — no placeholder text remains. If your rubric ever changes,
open `index.html`, find the `const RUBRIC = {` block near the top of the `<script>`
section, and edit the `definition`/`example` text for the relevant word and score.

Note: for **Ascend** and **Hazardous**, the original rubric didn't define separate
criteria for a score of 2 — the tool shows "No specific criteria defined for this score"
at that level as a placeholder for that gap. Let me know if you'd like criteria added
there.

## Export format

CSV exports are in **long format** — one row per participant, per word, per question —
with these columns:

| Column | Values |
|---|---|
| `participant_id` | the participant ID |
| `word` | one of the 12 target words |
| `session` | `pre`, `immediate` (the chapter/intermediate test), or `post` |
| `condition` | `high_preference` or `control` — carried over from whichever story (1 or 2) that word was assigned to as a target word; blank if the word wasn't a target word in either story |
| `question_type` | `definition` (rubric score 0–3), `remembered` (1/0, immediate session only), `pvt_choice` (1–4, post session only, only if seen in PVT), `pvt_accuracy` (0/1, post session only, only if seen in PVT) |
| `value` | the actual score/response for that row |

This keeps every data point on its own row rather than spreading everything across dozens
of per-word columns, so it's ready to pivot/filter directly in R, Python, or Excel.

## Condition (High Preference vs. Control)

On the Intermediate Test screen, choose a **Condition** for Story 1 and a Condition for
Story 2 (High Preference or Control) — this is saved with the participant and applied to
the `condition` column for every row involving that story's 4 target words, across all
three sessions (pre, immediate, post).

## PVT accuracy

For Post-Meaning, once you tick "Seen in PVT" for a word, you'll now see two separate
choices: **Picture selected** (Pic 1–4) and **Accuracy** (1 = Correct, 0 = Incorrect) —
both are entered directly by the scorer, since the tool doesn't have an answer key to
compute accuracy automatically.

## What it captures

- **Participant ID**
- **Pre-Meaning Test:** all 12 words, click each to see the rubric and select a score (0–3)
- **Intermediate Test — Story 1 / Story 2:** pick up to 4 target words per story, then for
  each: score against the rubric (0–3) and tick whether the participant remembered the
  target word from the story
- **Post-Meaning Test:** all 12 words, rubric score (0–3), plus a "Seen in PVT" checkbox
  and — if checked — which picture (1–4) the participant selected

## Publishing to GitHub Pages

1. Create a new GitHub repo (e.g. `meaning-test-coder`).
2. Add `index.html` to it.
3. In the repo settings, enable **GitHub Pages** for the `main` branch (root folder).
4. Your tool will be live at `https://<your-username>.github.io/<repo-name>/`

## Using it

1. Enter a Participant ID and click **Load / start participant**.
2. Score words in each section — click a word, review the rubric, click **Select** next
   to the score that fits.
3. For Story 1/2, pick your 4 target words first, then score just those.
4. For Post-Meaning, tick "Seen in PVT" and choose the picture number if applicable.
5. Export:
   - **Export this participant (CSV)** — one row for the current participant
   - **Export ALL participants (CSV)** — one row per participant, all combined
   - **Download full backup (JSON)** — everything in browser storage
   - **Restore from backup** — reload a JSON backup

## Notes

- Data lives only in the browser tied to the exact URL you use — always enter data through
  your one published GitHub Pages link, not downloaded local copies of the file, or data
  can appear to "go missing" across different local copies.
- The "Participants saved in this browser" panel shows exactly which participant IDs are
  currently stored, so you can verify nothing's missing before exporting.
- CSV export is in long format (see "Export format" above) — the same participant, word,
  and session combination always produces the same set of rows, so exports line up cleanly
  across participants even if different words were chosen as Story 1/2 targets.
