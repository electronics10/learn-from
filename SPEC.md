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
- **R5 · One owner per file.** Each workspace file has exactly one owning skill; the other only reads it. *Check:* the owner table in `shared/WORKSPACE.md`.
- **R6 · Portable workspace.** A workspace is plain files that work anywhere: local disk, a synced vault, a server. The dashboard opens from `file://` in any browser, with no build step and no server. *Check:* open `dashboard.html` from disk.
- **R7 · Dashboard is the lesson, chat is the conversation.** Teaching material goes on the dashboard; chat carries probes, grades and short replies (under ~80 words). *Check:* every dashboard update is followed by one chat message with a **Your turn** line that matches the dashboard.
- **R8 · Bounded detours.** A side topic is depth 1 and about 10 minutes; one that grows becomes a request for `syllabus`. *Check:* the side-topic rules in `study`.
- **R9 · One entry point.** The user starts everything with `/learn-from`; `syllabus` and `study` run only when the router or each other hands over. *Check:* skill descriptions and `disable-model-invocation` on the router.

## Non-goals

- **In-page Claude (interactive dashboard).** The dashboard does not call Claude itself. See decision D7.
- **Teaching a codebase line by line.** For code, the loop runs on the foundations the code relies on (maths, physics, algorithms), each from a proper source.
- **Building craft.** Projects that build real systems belong in a project; the plugin plans only the understanding they need.
- **Mixing many sources.** One main source and at most three supplements per workspace.

## Constraints

- Skills must work without a shell. A shell, when present, is used only for checks (JSON validation, task tests) and git.
- The dashboard uses only the browser: one HTML file, fonts and KaTeX from a CDN (math does not render offline).
- Workspace files are never written inside a git repository the user did not create for them (see D6).

## Decisions

| ID | Date | Decision | Reason |
|---|---|---|---|
| D1 | — | One main source plus up to 3 supplements; Claude-written material is the last resort and marked lower trust. | A coherent text teaches better than a patchwork, and Claude's own explanations can be wrong. |
| D2 | — | Multiple choice is used only for choices (continue/stop, hint/solution), never to test understanding. | Options turn recall into recognition. |
| D3 | — | The dashboard is a fixed renderer plus one JSON data block; Claude edits only the data. | Keeps edits small and safe, and the layout stable across workspaces. |
| D4 | — | Lessons on the dashboard are append-only. | The user can scroll back through earlier chunks. |
| D5 | — | The router is command-only; `syllabus` and `study` describe themselves as hand-over-only. | Ordinary questions must not start a study session. |
| D6 | — | A workspace is never placed inside a git repository unless the user says so. | Study files must not pollute code repositories. |
| D7 | 2026-10-09 | No in-page Claude calls on the dashboard. | The artifact `sample` capability works only when the page is published on claude.ai, not from `file://` (conflicts with R6). Grading in the page would copy the study rubric into page prompts and split progress between the page and `progress.md` (conflicts with R5). |

## Proposals

Ideas under consideration, not yet in the skills. Move one to Decisions when it is applied, or delete it when rejected.

- **P1 · Git history for a workspace (optional).** If `<topic>.study/` is its own git repository, `study` commits at session end. The user opts in by running `git init` in the workspace; nothing changes otherwise. Fits D6, which is about not writing into other repositories.
- **P2 · Split data out of the dashboard.** `dashboard.html` holds only the renderer; the state lives in a small `state.js`, and each unit's lessons in `lessons/<unit>.js`. Both load through `<script src>`, which works from `file://`. The renderer already shows only the current unit's lessons, so other units' lessons are dead weight in the file Claude reads. Side effect: renderer fixes reach old workspaces by copying `dashboard.html` again.
- **P3 · Read the dashboard data once per session.** Replace "read the current data block first" (before every write) with: read it at session start, and again only if the user or another session changed it.
- **P4 · Fewer dashboard writes.** Fold grade-only changes into the next write the user needs to read (a primer, fix, hint, solution, tasks, session end).
