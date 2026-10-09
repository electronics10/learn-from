# learn-from

Study from any source with one command, `/learn-from`.

- `/learn-from` — show all study workspaces and continue one.
- `/learn-from <book, topic or path>` — start or continue studying it.

Design intent and past decisions: [`SPEC.md`](SPEC.md).

## Parts

| Part | Job |
|---|---|
| `learn-from` (command) | Router: picks planning or studying. |
| `syllabus` (skill) | Records the intent; plans units along one main source (the spine), with supplements where they help; handles prerequisite gaps: insert a unit, open a separate track, or park. |
| `study` (skill) | The loop: primer → Feynman check → tasks (answer / auto / report, predict first) → grades → recall-first re-passes. Side topics are limited to depth 1 and ~10 minutes. |
| `shared/WORKSPACE.md` | The file format both skills follow. |
| `shared/dashboard.html` | The dashboard template (fixed renderer + one JSON data block). |

## Workspace

A folder `<topic>.study/` beside the main source (or where you choose) with `intent.md`, `syllabus.md`, `progress.md`, `dashboard.html` and `tasks/`. Open `dashboard.html` in a browser; ⌘R refreshes it.
