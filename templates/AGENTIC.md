# Project settings: <project name> (<tracker key>)

Copy this file to the root of the project's own repo as `AGENTIC.md`. It is loaded with `core/agreement.md`. It may add or tighten core rules. To loosen one, name the rule ID and give the human's reason under **Waived**.

Keep secrets out of it (C10). If the project repo is public, this file is public too.

* **Tracker:** <project key>, <parent epic if any>, <site>.
* **Repos and owning roles (C16):** <repo>: <role>.
* **Branches:** <feature branch pattern> → PR into <integration branch> → promoted to <release branch> by the human. Default branch: <name>.
* **Versions:** <scheme, and what earns a version>.
* **Context file (C12):** `.agentic-context/<role>.md`. Session snapshots go on <issue>.
* **Archiving (C17):** workers idle longer than <threshold> are archived. Archive folder: `<gitignored path>`.

## Sessions and roles

| Role (C3 tag) | Session or agent | Owns | Jira writes (C4) |
|---|---|---|---|
| `lead` | <session name> | <issues, areas> | <as core / narrower> |

## Tighter than core

* <extra C5 approval items>
* <extra C10 secret rules>

## Waived

Nothing.

## Shared contracts (C9)

* <APIs, env var names, schemas, CI checks, service names and ports>
