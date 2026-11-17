# agentic-instructions

A working agreement for projects where a human works with Claude agent sessions. Add it to any project; it holds nothing project-specific.

* `core/agreement.md`: rules for every project, with stable IDs (C1, C2, ...).
* `playbook.md`: step-by-step procedures for the rules. Not imported; agents read a section when they need it.
* `templates/CLAUDE.md`: the one pointer file a project repo holds (C23).
* `templates/session-context.md`: the context file a session keeps so a new session can pick up (C12).
* `CHANGELOG.md`: one entry per tag.

No secrets, tokens, project names or project settings belong in this repo. Project settings live on each project's settings page (C23).

## Using it in a project

1. Copy `templates/CLAUDE.md` to the project repo's root as `CLAUDE.md`, and fill in the tag and owner. Pointing at a new version is a C5 approval item.
2. Link a GitHub Project to the repo. Put anything that can't be discovered (C23) in the Project's README.

## Changing it

1. An agent proposes the change on a branch, as a PR that lists the rule IDs it changes and the version bump it needs.
2. Only the human merges.
3. After merge, tag the release and add a CHANGELOG entry.

Versions follow semver:

* **Major:** a rule removed or loosened; a rule's meaning changed so that sessions must act differently, even if the wording is close; or a format other code parses (like the C3 tag) changed.
* **Minor:** a new or stricter rule, or a new template.
* **Patch:** wording only.

## Rolling out a version

1. **Major only:** post a "Breaking change heads-up" (C9) on one issue in each consuming project, before tagging. Each project's lead acknowledges it.
2. Each project moves the tag in its `CLAUDE.md` in its own PR, approved by the human (C5).
3. After the merge, restart the project's sessions, subagents and teammates. Running sessions don't reload `CLAUDE.md`. Each lead updates its context file first (C12), so the new session picks up where it left off.

## Before tagging v1.0.0

* [ ] Sessions, cloud ones included, can fetch the agreement from the pointer's raw URL at session start.
* [ ] Sessions can read a GitHub Project's README with `gh` (needs the `project` scope).
