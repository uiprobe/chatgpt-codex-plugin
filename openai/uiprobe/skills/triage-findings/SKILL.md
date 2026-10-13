---
name: triage-findings
description: Go through a Uiprobe probe's findings with the user and decide, group by group, what to fix and what is intentional. Use when the user wants to triage or review findings, mark differences as intentional, or clean up a probe before someone fixes it. Dismisses findings only after the user confirms.
---

# Triage a probe's findings

The Uiprobe MCP server's instructions explain each tool and how to read a finding's evidence. Follow them. This skill walks the user through a probe's findings and records their decisions.

## Before you start

Dismissing a finding hides it for everyone who sees the probe, in the web app, on shared and public links and in CSV exports, and on future runs of the probe. It can always be restored. Tell the user this once, at the start.

## Steps

1. **Find the probe.** If the user gave a probe or viewer URL, use it. Otherwise call `list_probes`, and ask which probe if more than one fits.
2. **Get all findings.** Call `get_probe` and page through with `nextOffset` until you have them all. If the latest run is still `PENDING` or `RUNNING`, say so and offer to wait: findings can only be dismissed once the run has finished.
3. **Go through them in batches of up to five**, groups first, then single property findings, then spacing, then unverified. For each, give one line: what differs, the design value and the page value, and the `findingUrl`. Number them so the user can answer by number. Offer images through `get_finding_images` when a line isn't clear enough.
4. **Ask for a decision per item:** fix, intentional, or skip. For a group, ask whether the decision covers the whole group or only some of its findings.
5. **Confirm before dismissing.** After each batch, list the findings marked intentional and ask once to dismiss them. Then dismiss them with `dismiss_findings`. Dismiss a whole group in one call when the user chose the whole group.
6. **Repeat** until every finding has a decision or the user stops.

## End with

- **To fix:** each finding with its design value, page value and `findingUrl`. In a coding agent, offer to continue with the `uiprobe:match-design` skill. Elsewhere, offer to write the list as tickets.
- **Dismissed:** what was dismissed, and that any of it can be restored by asking.
- **Skipped:** what is left undecided.
- The probe's viewer URL.

## Restoring

When the user wants a dismissed finding back, call `get_probe` with `includeDismissed` to get the dismissed findings' ids, then `restore_findings`, and confirm. Restoring works per finding, also for findings that were dismissed as part of a group.

## Don't

- Dismiss anything the user didn't confirm in this conversation.
- Suggest dismissing a finding because it looks small. Whether a difference matters is the user's call.
- Describe the list as shorter or the findings as fewer after triage. Dismissed findings are set aside, not removed.
