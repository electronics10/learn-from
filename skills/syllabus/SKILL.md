---
name: syllabus
description: Plans a study workspace for the learn-from plugin. Use only when the /learn-from router or the study skill hands over a setup or a plan revision; never for ordinary questions.
---

# syllabus

Plan what to study, in what order, from which sources. You own `syllabus.md` and nothing else (except the `[open]` → `[done: …]` marker in progress.md). The format is in `../../shared/WORKSPACE.md` (relative to this skill's base directory); read it first.

**Principle: the source teaches.** A plan is a list of **units**, each pointing to a span of a high-trust source (book pages, lecture, code lines). One **main source** is the **spine**: it sets the order and the notation. Use as many supplements as make units faster or clearer, but the plan never jumps from source to source. Claude-written material is the last resort and is marked lower trust.

## Setup (no workspace yet)

1. **Goal.** Ask for an optional one-line goal ("need ch. 5–7 for project X"). Default: follow the source.
2. **Sources.**
   - The user named a source → use it as main. Read its table of contents and preface (Mathpix markdown if present, else the PDF).
   - No source → search the web for high-trust options (standard textbooks, university courses, official docs). Propose **one main source** (the spine) and any supplements that make specific units faster or clearer, one line each on why. Confirm with the question tool before planning.
3. **Workspace.** Create `<topic>.study/` by the location rule in WORKSPACE.md.
4. **Prerequisite check.** List 3–6 prerequisites the main source assumes. Ask one short question for each, one at a time, in chat (free text, not multiple choice). Record solid / shaky / gap. A gap that the source does not cover becomes a unit before the first unit that needs it.
5. **Units.** Build the plan with the rules below; write `syllabus.md`.
6. **Show the plan** in chat as a short numbered list (unit, source span, one-line why) and hand back to `/learn-from`, which routes to `study`. study builds the dashboard.

## Planning rules

- **Unit size**: what one to three sessions can cover; usually a chapter of a well-written book, a lecture, or a module of code.
- **Order (the spine)**: respect `needs:`; otherwise keep the main source's order. A linear textbook needs no planning beyond its contents in order. A supplement may teach a unit or part where it is faster or clearer; name it in that unit's line with why. The unit keeps its place in the spine.
- **Goal-driven sources** (reference books like a mathematical-methods handbook, large docs): plan a **survey unit** first (what each chapter is for and when to reach for it, read lightly) and then only the units the goal needs, each with its prerequisites.
- **Hands-on goals** (machine learning, antenna design): pick sources with exercises or projects; note in the unit line which units need tools (Python, a solver), so study can plan tasks. A goal of *craft* (building real systems) belongs in a project; say so, and plan only the understanding it needs.
- **Codebases**: do not plan the code itself. Plan the foundations it relies on (the maths, physics or algorithms), each from a proper source.
- Give every unit a one-line **why**. A unit whose why you cannot state does not belong in the plan.

## Revise (a request is open, or the user asks)

1. Read `syllabus.md`, `progress.md` and the open requests.
2. For a **prerequisite gap**, estimate its size and propose with the question tool, recommended option first:
   - **Insert a unit**: about 3 or fewer new core ideas, fits one session, a source you already have covers it.
   - **Separate track**: a whole subject. Name a specific outside source (book + chapters). Create `<subject>.study/` with its own syllabus, add it under `## Tracks`, and mark the waiting unit as paused.
   - **Park it**: not on the path to the goal. Add it under `## Parked` with the reason.
3. For a user change ("skip ch. 4", "now I need Y"), edit the units, keeping IDs stable.
4. Bump `Plan: v<n>` with the date and reason; mark handled requests `[done: <decision>]` in progress.md; hand back to `/learn-from`.

Revise only at these checkpoints. Do not reshuffle the plan between them.
