# learn-from — Spec

The intent behind the plugin: what it must do, what it must not do, and why it is built this way. Edit this file when the intent changes; the skills and `shared/WORKSPACE.md` follow from it.

## Purpose

Help one person understand a subject from a high-trust source (a textbook, a lecture course, documentation) in the least time that still leaves them able to explain each idea in their own words. Claude plans, primes, probes and checks; the source teaches.

## Requirements

Each requirement names how to check it.

- **R1 · Source first.** Every unit points to a span of a named source (chapter and pages, lecture, code lines). Units without one say `source: Claude-written` and show as lower trust. *Check:* every unit line in `syllabus.md` has a span or the marker.
- **R2 · Understanding is shown, not assumed.** Every core idea of a studied unit gets a grade (solid · shaky · gap) from a Feynman check the user answers in free text. *Check:* no idea of a finished pass is left `todo`; no understanding check uses multiple choice.
- **R3 · Least time.** Chunk size and support level adapt to the grades; parts can be skimmed or skipped. *Check:* the adapt rule in `study` changes chunk and support after each chunk.
- **R4 · Sessions survive breaks.** A new session resumes from the workspace files alone, with no memory of the last chat. *Check:* `progress.md` is written after every chunk and every task, and holds the open questions at session end.
- **R5 · One writer per file.** Only one skill may write each workspace file: `syllabus` writes `syllabus.md` and `intent.md`; `study` writes `progress.md`, `dashboard.html` and `tasks/`. The other skill may read the file but not change it. The one exception is the `[open]` → `[done: …]` marker, which `syllabus` sets in `progress.md`. This keeps the two skills from overwriting each other's changes, and makes it clear which skill to fix when a file is wrong. *Check:* the `owner:` labels in `shared/WORKSPACE.md`.
- **R6 · Portable workspace.** A workspace is plain files that work anywhere: local disk, a synced vault, a server. The dashboard opens from `file://` in any browser, with no build step and no server. *Check:* open `dashboard.html` from disk.
- **R7 · Dashboard is the lesson, chat is the conversation.** Teaching material goes on the dashboard; chat carries probes, grades and short replies (under ~80 words). *Check:* every dashboard update is followed by one chat message with a **Your turn** line that matches the dashboard.
- **R8 · Bounded detours.** A side topic is depth 1 and about 10 minutes; one that grows becomes a request for `syllabus`. *Check:* the side-topic rules in `study`.
- **R9 · One entry point.** The user starts everything with `/learn-from`; `syllabus` and `study` run only when the router or each other hands over. *Check:* skill descriptions and `disable-model-invocation` on the router.
- **R10 · The original intent is kept.** At setup, `syllabus` records why the user is studying this, in their own words, in `intent.md`. The original entry is never edited; later changes are added below it with a date. *Check:* every workspace has `intent.md` with its `## Original` section unchanged since setup.

## Non-goals

- **Teaching a codebase line by line.** For code, the loop runs on the foundations the code relies on (maths, physics, algorithms), each from a proper source.
- **Building craft.** Projects that build real systems belong in a project; the plugin plans only the understanding they need.
- **Jumping between sources.** Any number of sources may be used, but the plan follows one clear, systematic spine (D1); it is never a patchwork that hops from book to book.

## Constraints

- Skills must work without a shell. A shell, when present, is used only for checks (JSON validation, task tests).
- The dashboard uses only the browser: one HTML file, fonts and KaTeX from a CDN (math does not render offline).

## Decisions

| ID | Date | Decision | Reason |
|---|---|---|---|
| D1 | 2026-10-09 | One main source is the **spine**: it sets the order of units and the notation. Supplements have no fixed limit; each is used where it makes a unit or part faster or clearer, and is named in that unit's line with why. Claude-written material is the last resort and marked lower trust. | Several good sources make learning faster, but a plan that jumps between them loses its structure; Claude's own explanations can be wrong. |
| D2 | — | Multiple choice is used only for choices (continue/stop, hint/solution), never to test understanding. | Options turn recall into recognition. |
| D3 | — | The dashboard is a fixed renderer plus one JSON data block; Claude edits only the data. | Keeps edits small and safe, and the layout stable across workspaces. |
| D4 | — | Lessons on the dashboard are append-only. | The user can scroll back through earlier chunks. |
| D5 | — | The router is command-only; `syllabus` and `study` describe themselves as hand-over-only. | Ordinary questions must not start a study session. |
| D6 | 2026-10-09 | The dashboard footer suggests `git init` and committing after sessions, for users who want a history. | Commits stay under the user's control. |
| D7 | 2026-10-09 | `study` reads the dashboard data once per session, and again only when someone else may have changed it (the user edited the file, or another session ran). | It used to re-read the whole, growing JSON before every update (about 10 times a session), although Claude made every later change itself. |
| D8 | 2026-10-09 | `syllabus` writes `intent.md` at setup (R10). `syllabus.md` keeps its one-line `Goal:`; `study` does not read `intent.md`. | The `Goal:` line is short and gets revised; the original reasons guide later plan revisions (insert, track or park) and show how the aim changed. `study` does not need it, so it adds no reading per session. |

## Proposals

Ideas under consideration, not yet in the skills. Move one to Decisions when it is applied, or delete it when rejected.

- **P2 · Split data out of the dashboard** (deferred: the user is testing it first). `dashboard.html` holds only the renderer; the state lives in a small `state.js`, and each unit's lessons in `lessons/<unit>.js`. Both load through `<script src>`, which works from `file://`. The renderer already shows only the current unit's lessons, so other units' lessons are dead weight in the file Claude reads. Side effect: renderer fixes reach old workspaces by copying `dashboard.html` again.
