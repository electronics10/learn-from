# learn-from — notes for agents

Read `SPEC.md` before changing anything: it holds the requirements, non-goals and the reasons behind past decisions.

Each kind of change has one home:

| Change | Edit |
|---|---|
| Intent, requirements, decisions, proposals | `SPEC.md` |
| Workspace file formats and file owners | `shared/WORKSPACE.md` |
| What a skill does, step by step | `skills/<name>/SKILL.md` |
| Dashboard layout and rendering | the renderer in `shared/dashboard.html` (never the sample data's shape without `WORKSPACE.md`) |

A change that breaks a requirement in `SPEC.md` needs the spec edited first, with a decision row saying why.
