# Study workspace format

The contract between the `syllabus` and `study` skills. Each file has exactly one **owner**; the other skill only reads it. Change this file, not the skills, when the format changes.

## Location

A workspace is a folder named `<topic>.study/`.

- **Default**: beside the main source file (the book's PDF or Markdown).
- **No source file** (a course website, a codebase, a topic): ask the user once where to put it.
- Any path works: local disk, a synced vault, or a server. Nothing depends on the user's own computer.

```text
<topic>.study/
├── syllabus.md      # owner: syllabus
├── progress.md      # owner: study
├── dashboard.html   # owner: study (copied from shared/dashboard.html)
└── tasks/           # owner: study (code, simulations, tests, user reports)
```

## syllabus.md (owner: syllabus)

```md
# Syllabus — <topic>
Goal: <one line | follow the source>
Plan: v2 · 2026-10-07 · <reason for the last revision>

## Sources
- [main] Wasserman, *All of Statistics* — ../all-of-statistics.pdf (+ .md) — one coherent rigorous text
- [supp] Blitzstein & Hwang, *Introduction to Probability* — free PDF — gentler second explanation

## Prerequisites
- Multivariable calculus: solid
- Linear algebra: shaky (eigenvectors)

## Units
1. u01 · Probability — main ch. 1 (pp. 3–20) · needs: — · why: foundation for everything
2. u02 · Random variables — main ch. 2 · needs: u01
3. u07 · Complex analysis essentials — supp ch. 3 §1–4 · needs: — · why: inserted for u05 (gap)

## Parked
- Measure-theory intuition — wanted by u01 Note · park reason: not on the goal path

## Tracks
- complex-analysis.study — separate track opened for u05; resume u05 after its unit 4
```

Rules:

- **Unit IDs are stable** (`u01`, `u02`, …). New units get the next free number, wherever they are placed in the order. Never renumber.
- Every unit names a **source span** (book chapter and pages, lecture, code lines). A unit with no source span says `source: Claude-written` and is treated as lower trust.
- `needs:` lists unit IDs that must be studied first.
- The order of the list is the study order.

## progress.md (owner: study)

```md
# Progress — <topic>
Chunk: M · Support: L2 · Updated: 2026-10-07

## Open questions
- Why must B'(s) be parallel to N(s)? (u03 · Frenet frame · 2026-10-07)

## Requests for syllabus
- [open] Complex analysis needed for u05 (contour integrals). Size: whole subject. Seen: u05 chunk 2.

## u01 · pass 1 · 2026-10-07
Parts: 1.1 done skim · 1.2 done close · 1.3 now close · 1.4 skim · 1.8 skip · chunk 2 of 3
Ideas:
- Sample space; events as sets: solid (2026-10-07)
- Axioms and consequences: shaky (2026-10-07) — cannot say why the proof builds disjoint B_i
Tasks: 1.1 done · 1.3 hint · Sim1.1 open (auto) · open: 1.2 1.4–1.12
Side topics: independence vs disjointness (solid, 2026-10-07)
```

Rules:

- One section per unit that has been started, titled `## <unit id> · pass <n> · <date>`.
- Idea status: `solid` · `shaky` · `gap` · `todo`. Non-solid ideas carry a one-line reason; the next pass probes that reason first.
- Task status: `open` · `done` · `hint` (solved after a hint) · `solution` (solution seen). Hands-on tasks note their check: `(answer)`, `(auto)`, `(report)`.
- `## Requests for syllabus`: written by study when a side topic grows into a prerequisite gap. Each entry starts `[open]`; syllabus turns it into `[done: <what was decided>]` (syllabus may edit only this marker in progress.md).

## tasks/ (owner: study)

- One file or folder per hands-on task, named `<unit id>-<slug>` (e.g. `u01-coins.py`, `u05-dipole/`).
- `auto` tasks ship with their check: a test file or a script that prints PASS or FAIL against the target.
- `report` tasks get a `<name>.report.md` where the user's observations and the prediction are kept.

## dashboard.html (owner: study)

Copy `shared/dashboard.html` once into the workspace. Afterwards edit **only** the JSON inside `<script type="application/json" id="data">`. The renderer handles layout, counts and long plans.

Fields (the template's sample shows each one):

| Field | Content |
|---|---|
| `title`, `goal`, `chunk`, `support`, `updated` | header |
| `turn` | `{step, text}`: the one thing the user does now. Always set; matches the chat's **Your turn**. |
| `side` | `{topic, from}` while a side topic is open, else `null` |
| `request` | `{text}` while a request for syllabus is open, else `null` |
| `queue` | open questions first, then assigned tasks not yet done; at most 5. `{key, text, on}` |
| `units` | every unit in syllabus order: `{id, title, source, state: done|now|todo|parked, pass, last, shaky, gap}`; `source` empty for Claude-written units |
| `current` | `{unit, chunk, chunks, parts, ideas}`. `parts`: `{id, title, mode: close|skim|skip, state: done|now|(omit)}`; every part of the current chunk is `now`. `ideas`: `{text, status, why, now}` |
| `lessons` | `{"<unit id>": [{chunk, title, blocks}]}`. Append only. Block types: `primer`, `example`, `note`, `fix`, `hint`, `solution`, `side` (text, with `**bold**`, `*italic*`, `` `code` ``, `- ` bullets, blank-line paragraphs); `svg` (`{svg, caption}`); `task` (`{key, check: answer|auto|report, text, predict, file}`) |

JSON rules: double every backslash (`\\( \\kappa \\)`), escape double quotes, no `</script` inside strings. When a shell is available, check after each write:

```bash
python3 -c "import json,re,sys;json.loads(re.search(r'id=\"data\">(.*?)</script>',open(sys.argv[1]).read(),re.S).group(1))" dashboard.html
```

SVG: single-quoted attributes; strokes use class `ink` or `acc` so they follow the theme.

Viewing: open the file in a browser and press ⌘R after updates. On a server, copy it or serve the folder (`python3 -m http.server`) over an SSH port forward.
