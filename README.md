# learn-from

Study from any source with one command, `/learn-from`.

- `/learn-from` — show all study workspaces and continue one.
- `/learn-from <book, topic or path>` — start or continue studying it.

## Parts

| Part | Job |
|---|---|
| `learn-from` (command) | Router: picks planning or studying. |
| `syllabus` (skill) | Plans units from a main source (plus up to 3 supplements); handles prerequisite gaps: insert a unit, open a separate track, or park. |
| `study` (skill) | The loop: primer → Feynman check → tasks (answer / auto / report, predict first) → grades → recall-first re-passes. Side topics are limited to depth 1 and ~10 minutes. |
| `shared/WORKSPACE.md` | The file format both skills follow. |
| `shared/dashboard.html` | The dashboard template (fixed renderer + one JSON data block). |

## Workspace

A folder `<topic>.study/` beside the main source (or where you choose; never inside a git repo unless asked) with `syllabus.md`, `progress.md`, `dashboard.html` and `tasks/`. Open `dashboard.html` in a browser; ⌘R refreshes it.
