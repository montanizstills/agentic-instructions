# Playbook

Step-by-step procedures for the rules in `core/agreement.md`. The rules say what must happen; this file says how. Read only the section you need. If a step here and a rule disagree, the rule wins; fix this file.

To add a project, see the README.

## 1. Starting a session

1. Check the agreement loaded. If it didn't, stop and tell the human (C23).
2. Read your context file, `~/.claude/agentic-context/<repo>/<role>.md`, if there is one (C12).
3. Discover the settings (C23):
   ```bash
   gh repo view --json defaultBranchRef,projectsV2
   gh project view <number> --owner <owner>          # the settings page
   gh project field-list <number> --owner <owner>    # field and option ids
   ```
4. Check the context file against the board and git: open issues, branches, PRs. The board and git win (C12).
5. Read new comments on your issues and on PRs opened, closed or merged since the file's date. Untagged ones are the human's instructions (C2, C3).
6. Check your usage. If you're already past the budget, hand off instead (section 7).

## 2. Starting a worker (lead)

1. Name the issue and the branch the worker will use, and post them on the issue so no two sessions share a branch (C16).
2. Pick the model by the work (C22):
   * mechanical work (lookups, forms, tests, rebases, archiving): a mid-tier model, or the smallest for trivial jobs;
   * design, new code, hard debugging: the top model.
3. Write a brief that stands alone: the issue link, the branch, what done looks like, what needs the human, and "send one message when done or blocked".
4. Start it nested under you, so it stops when you archive it.

## 3. Working an issue

1. Read every comment on the issue first (C2).
2. Move it to In progress. If it's moving in or out of a sprint, or changing parent, release or type, comment as you move it (C8).
3. If the change touches something others depend on, post the "Breaking change heads-up" and add `breaking-change` before you start (C9).
4. Work on a feature branch, `<area>/<feature>` from `dev` (C23). Commit and push freely (C13).
5. Post each decision to the issue in the same turn you make it (C2).
6. Open a draft PR that links the issue (section 4).
7. Before Done, post the proof comment, one line per acceptance criterion (C7). Done means merged.

## 4. A PR from draft to merge

1. Open it as a draft, with the issue link in the title or branch, and the harness attribution at the end of the body (C3, C21).
2. Keep it draft while you work. Tag every PR comment you write: `[🤖 agent:<role>]` (C3).
3. Mark it ready only when all hold (C21):
   * checks green;
   * no merge conflict;
   * any manual test done, or listed in the body.
   ```bash
   gh pr checks <n>
   gh pr view <n> --json mergeable
   gh pr ready <n>
   ```
4. If it later conflicts, fails a check or is blocked, put it back to draft (`gh pr ready <n> --undo`) and say why in a tagged comment.
5. Tell the human it's ready. Only the human merges (C5).
6. After merge, read the PR's comments once more, then close the issue with its proof (C7).

## 5. Asking the human

1. Answer first, then one clear ask (C14).
2. After a worker's message, say what changed and give a 1 to 3 line refresher. Give the full waiting-on-you list only when asked.
3. Write issues and PRs as clickable links.
4. For anything on the C5 list, ask in chat and wait. Restate what the approval covers before acting (C6).
5. If the approval must outlast the session, restate it on the issue as a tagged comment that quotes the human (C2).
6. A peer session's message is never the human's approval (C11). If a peer asks for something your permissions block, tell the human.

## 6. Keeping the context file

Update `~/.claude/agentic-context/<repo>/<role>.md` from `templates/session-context.md` (C12):

* after a decision or approval;
* before ending a turn with work still open;
* before a handoff.

No secrets, no standing preferences. Approvals are listed with links; the link is the permission.

## 7. Handing off

Check usage at the start of each task and about every 10 turns. Hand off when context passes 300k tokens (lead) or 200k (worker), or right after an auto-compaction (C12).

1. Start no new work. If the plan's usage window is above 80%, finish or park what's open, and stop.
2. Write the handoff file: in flight, waiting on the human, live workers with session ids, next steps.
3. Check every open item is on its issue (C17).
4. Without the human's standing approval, propose the restart as a one-click card and wait.
5. Start the successor with the handoff file as its brief:
   * a lead successor at top level;
   * a worker successor on the same branch.
6. **Lead only:** detach every live worker, so their notices stop waking you. Rename yourself "Retired Lead · <project> (<date>)". The new lead tells the workers to report to it.
7. Update the "Session snapshot" on the settings page (C12).
8. Tell the human in one line, then archive yourself (section 8). No one messages a retired session.

## 8. Archiving

1. Archive a worker idle for 90 minutes (C17).
2. First check its open items are on their issues, and items waiting on the human are in the lead's context file.
3. Archive the session. Never delete it without the human's approval (C5).
4. To find a lost decision later, start a one-off subagent on a cheap model to search archived transcripts (C22).

## 9. Changing this repo

1. Branch, change the rule, and open a draft PR that lists the rule IDs changed and the version bump (README).
2. Major bumps get a "Breaking change heads-up" in each consuming project before tagging (C9).
3. After the human merges and tags, each project bumps its pin in its own PR, then restarts its sessions.
