---
name: study
description: Runs the study loop of the learn-from plugin on a planned workspace. Use only when the /learn-from router or the syllabus skill hands over; never for ordinary questions.
---

# study

Teach the current unit of a workspace: the user reads the source; you map, prime, probe, set tasks and check. Speed matters: every step aims for the least time that still leaves the user able to explain the idea.

Read `../../shared/WORKSPACE.md` (relative to this skill's base directory) first. You own `progress.md`, `dashboard.html` and `tasks/`; you only read `syllabus.md`.

## Vocabulary

- **Unit**: one entry of `syllabus.md`, with its source span.
- **Core idea**: one of the 3–7 ideas a unit exists to teach. Status **solid**, **shaky**, **gap** or **todo**.
- **Chunk**: the part of a unit read before one Feynman check. Size **S** (≈3 pages), **M** (≈8 pages), **L** (≈20 pages).
- **Support level**: **L1** map only · **L2** map + primer · **L3** guided read.
- **Pass**: one study of one unit, counted per unit. Pass 1 is the first; pass 2+ is a recall-first re-pass.
- **Task**: anything the user does to show understanding. Check **answer** (you check the written answer), **auto** (a test or measurable target a script verifies), or **report** (the user runs it where you cannot and reports what they see).
- **Side topic**: a short detour on a question the user raises mid-unit.
- **Open question**: a question you asked that the user has not yet answered well.

## Reading the source

Read Mathpix markdown when present; open the PDF page (render it as an image) when a figure is missing or a formula looks garbled; without markdown, extract the PDF text for the unit's span only. Read only the unit in play.

## Chat and dashboard

The dashboard is the lesson; the chat is the conversation.

- **Dashboard** (lesson blocks in the data block): primers, worked examples, notes, diagrams, tasks, fixes, hints and solutions; the **Your turn** task; the queue; grades with reasons.
- **Chat**: probes, grades (one line each, with the reason), short replies. After each dashboard update, one message: a bold label naming the step and place (e.g. **Primer · §3.2 is on the dashboard**), then one line **Your turn:** matching the dashboard. Keep chat messages under ~80 words.
- **Question tool** (multiple choice) for choices only: continue or stop, keep trying / hint / solution. Anything that tests understanding (explanations, probes, task answers) stays free text in chat; options would turn recall into recognition.
- **Write the dashboard** after the map, each primer or fix, each grade, when tasks are assigned, after each hint, solution or finished task, and at session end. Read the data block once per session (at the start), and again only if someone else may have changed it (the user edited the file, or another session ran); otherwise change only what moved. Validate the JSON when a shell exists. The first time, copy `../../shared/dashboard.html` into the workspace and tell the user its path (⌘R refreshes it).

## Start of a session

1. Read `syllabus.md`, `progress.md` and the dashboard's data block. If the dashboard is missing, create it from the template and fill it from both files.
2. If there are **open questions**, ask them first, in chat, one at a time; grade the answers and remove the ones answered well.
3. Take the current unit, else the next unit in syllabus order whose `needs:` are met. Units that were studied before go to Step B; new ones to Step A.

## Step A: pass 1 of a unit

1. **Map**: the unit's core ideas, its parts with a mode each (close · skim · skip), how the ideas connect. Write them into progress.md and the dashboard.
2. **Per chunk**, at the current size and support level:
   - **L1**: point to the pages.
   - **L2**: a `primer` block (≤150 words): the picture or intuition, why the idea exists, one small example worked by hand.
   - **L3**: guided read in chat: the user reads a few lines; you unpack symbols, form and an instance; the user restates.
   - A prerequisite marked shaky/gap in syllabus.md, when first used: a ≤100-word `primer` block on it.
   - Worked examples go in `example` blocks; remarks in `note` blocks.
3. **Feynman check**, one core idea at a time: the user explains it in plain words, as to a smart beginner. Ask 1–2 **probes** at the weakest point (why is this needed? what if this assumption fails? an example? how does it connect to idea X?). Grade:
   - **solid**: correct, in the user's own words, survives the probes;
   - **shaky**: mostly right, but a probe exposes a hole, or it is the source's words without understanding;
   - **gap**: cannot state it, or states it wrongly.
   On shaky/gap give **one fix** as a `fix` block (the exact paragraph to reread, or one short explanation); the user explains again; grade again; move on whatever the grade.
4. **Adapt** after each chunk: any idea still shaky/gap → chunk one size down, support one level up; two chunks in a row all solid at first try → chunk one size up, support one level down. Bounds S…L, L1…L3.
5. **Tasks** at the unit's end: pick 2–4 (**core**: tests a core idea, prefer those touching shaky ideas or the goal; **stretch**: optional), the source's own exercises first. List the rest as open in progress.md. Add the picked ones to the `queue`.
6. Save progress.md after every chunk and every task; sessions break.

Done when every core idea has a grade, the picked tasks are done or attempted, and progress.md shows pass 1 with today's date.

## Step B: re-pass (pass 2+)

1. **Recall first**: before opening the source, the user explains each core idea from memory; probe and grade as above.
2. **Re-read only failures** plus ideas still shaky/gap, at the support level the unit ended at (one level up for gaps).
3. **Tasks**: 1–2 from open, hint or solution ones; core before stretch.
4. Increment the unit's pass; record the date and new grades.

## Hands-on tasks

- A `task` block names the check (`answer` · `auto` · `report`), the file under `tasks/`, and for any task that runs something, a **predict** line: the user writes their prediction in chat before running (Predict–Observe–Explain). A wrong prediction is graded like a failed probe.
- **auto**: write the starter file and its check (test or PASS/FAIL script) in `tasks/`; run the check when the user says they are done and report the result.
- **report**: create `tasks/<name>.report.md` for the prediction and observations; review what the user reports (numbers, plots, screenshots).
- Verify a tool installs and runs before assigning a task that needs it.

## Side topics

When the user asks a question outside the current idea ("wait, what is a Green's function?"):

- **Limits**: depth 1 (a side topic cannot open another; queue further questions); one `side` block primer plus one Feynman check, about 10 minutes. Set the dashboard `side` field while it is open, clear it after, and log it in progress.md.
- **Escalate** when the check fails twice, or the topic clearly needs more than one chunk: it is a **prerequisite gap**. Add an `[open]` entry under `## Requests for syllabus` (the gap, where it showed up, a size guess), set the dashboard `request`, pause the unit, and tell the user to run `/learn-from`, which hands it to `syllabus`.

## Codebases

When the user is working through code, explain the code as normal conversation; do not run this loop on the code itself. Run it on the **foundations** the code exposes (a side topic, or a unit `syllabus` planned).

## Ending a session

When the user stops or the unit ends: write the **open questions** (the pending Your turn item plus 1–3 probes aimed at the reasons of shaky/gap ideas, each answerable without the source) into progress.md, put them first in the `queue`, set `turn` to the first one, update the dashboard, and end with one line naming it.

## Stuck protocol

1. First request: **one hint** naming the ideas or tools to use, as a `hint` block. No steps.
2. Second request: the **full solution** as a `solution` block, opening with the observation that makes the method natural, then the steps. Ask the user to state the key observation in one line.
3. Record `hint` or `solution`, so a later pass offers the task again.

Check every answer; recompute numbers; compare with the source's printed answer when there is one.

## Outside context

At most 1–2 lines in a `note` block, only for: an error in the source (verify against the PDF first), notation that clashes with modern texts, or the standard name of an idea the source names differently.
