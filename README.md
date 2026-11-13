# agentic-instructions

A working agreement for projects where a human works with Claude agent sessions. Add it to any project; it holds nothing project-specific.

* `core/agreement.md`: rules for every project, with stable IDs (C1, C2, ...).
* `templates/AGENTIC.md`: the project settings file. Each project copies it to its own repo and fills it in.
* `templates/session-context.md`: the context file a session keeps so a new session can pick up (C12).
* `templates/archive-summary.md`: the Archivist's summary of an archived worker (C17).
* `CHANGELOG.md`: one entry per tag.

No secrets, tokens, or project details belong in this repo.

## Using it in a project

Adding the submodule is a C5 approval item. Add it pinned to a tag:

```bash
git submodule add https://github.com/montanizstills/agentic-instructions .agentic
git -C .agentic checkout v1.0.0
cp .agentic/templates/AGENTIC.md AGENTIC.md   # then fill it in
echo ".agentic-context/" >> .gitignore
git add .gitmodules .agentic AGENTIC.md .gitignore
git commit
```

Then the project's `CLAUDE.md` starts with:

```
@.agentic/core/agreement.md
@AGENTIC.md
```

followed by anything that only matters to that session, including: "If `.agentic/` is empty, stop and tell the human."

Fresh clones need `git clone --recurse-submodules`, or `git submodule update --init` after cloning.

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
2. Each project bumps its pin in its own PR, approved by the human (C5):
   ```bash
   git -C .agentic fetch --tags
   git -C .agentic checkout v1.1.0
   git add .agentic
   git commit
   ```
3. After the merge, restart the project's sessions, subagents and teammates. Running sessions don't reload `CLAUDE.md`. Each lead updates its context file first (C12), so the new session picks up where it left off.

## Before tagging v1.0.0

* [ ] Claude Code resolves `@.agentic/...` imports from a project's `CLAUDE.md` when the files live in a submodule.
* [ ] Cloud and remote sessions clone with submodules. If not, the "`.agentic/` is empty" line in `CLAUDE.md` covers it.
