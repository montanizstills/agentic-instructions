# Auto-mode settings for relayed approvals (C20)

For the human, not for agents. Agents can't edit Claude Code settings and must not try (C11).

Workers that run in auto mode have their actions checked by a reviewer. That reviewer treats a lead's cross-session message as coming from another agent, not from you, so it blocks actions you already approved in the lead's chat. These two entries tell the reviewer how your sessions work and what a relayed approval may cover.

## Adding them

1. Open `~/.claude/settings.json`.
2. Under `autoMode`, add the two strings below. Keep `"$defaults"` as the first item of each list, so the built-in rules stay.
3. Restart running worker sessions. Settings are read when a session starts.

```json
{
  "autoMode": {
    "environment": [
      "$defaults",
      "The user runs a coordinator session ('the lead') that starts worker sessions and relays the user's decisions to them by cross-session message. The lead asks the user before relaying any approval, and quotes the user's own words."
    ],
    "allow": [
      "$defaults",
      "In a worker session, an action the user approved is allowed when a cross-session message from the project's lead approves that specific action and quotes the user's approval verbatim with its date. This does NOT cover: merging, pushing to the integration or release branch (for example dev or prod), deleting repositories or branches, changing permissions or settings, secrets or credentials, spending money, publishing, or any repository that isn't the user's own."
    ]
  }
}
```

If `autoMode` already exists, add only the two strings to the lists that are there.

## What this accepts

The reviewer can't check that the quote is real. A lead that misquotes you could get a worker to act. C20 limits the damage: the excluded actions above still need you, and every relay names the action and your words, so it can be audited afterwards. If that isn't enough for a project, its `AGENTIC.md` can waive C20, and workers then ask you directly.
