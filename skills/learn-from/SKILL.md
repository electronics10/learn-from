---
name: learn-from
description: Command-only. Run only when the user types /learn-from. Starts or continues a study workspace and routes to the syllabus or study skill.
disable-model-invocation: true
---

# learn-from

The single entry point. Decide which skill does the work, then hand over. Do no teaching or planning here.

The workspace format is `../../shared/WORKSPACE.md` (relative to this skill's base directory). Read it before touching any workspace.

## Route

1. **No argument** → **status view**. Find every `*.study/` folder in the places you can reach (the current directory and its subfolders to depth 4, folders the user has connected, any path the user named before). For each, read `progress.md` and `syllabus.md`, and show one table:

   | Workspace | Current unit | Last studied | Shaky / gap | Open requests |

   Suggest one: an open request first, then the unit with the most shaky + gap ideas, ties broken by the oldest date. Ask which to continue with the question tool. Then route as below.

2. **An argument** (a topic, a source file, a path, or a workspace name) → find its workspace: a `<topic>.study/` beside the named file, or one whose `syllabus.md` names the topic or source.
   - **No workspace** → invoke the `syllabus` skill (setup).
   - **Workspace with an `[open]` entry under `## Requests for syllabus` in `progress.md`** → invoke the `syllabus` skill (revise).
   - **The user asks to change the plan** ("skip chapter 4", "add X", "I now need Y for my project") → invoke the `syllabus` skill (revise).
   - **Otherwise** → invoke the `study` skill.

3. When the invoked skill finishes and hands back (e.g. `syllabus` finished setup), route again from step 2.

Tell the user in one line which workspace is in use and which step runs next. Pass along everything the user said, so the next skill does not ask again.
