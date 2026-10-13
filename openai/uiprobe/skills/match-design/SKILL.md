---
name: match-design
description: Bring a webpage in line with its Figma design using Uiprobe. Use when the user asks to check a page against a Figma frame, fix design differences, do visual QA, or make an implementation "match the design", and gives (or has) a Figma URL and a page URL. Needs a code repository, for example in Codex or Claude Code.
---

# Match a page to its Figma design

The Uiprobe MCP server's instructions explain each tool, how to poll a run, how to expose a local page and how to read a finding's evidence. Follow them. This skill adds the loop around those tools.

## The loop

1. **Find or create the probe.** Call `list_probes` first. If a probe already exists for this design and page, use `rerun_probe` on it so its history is kept. Otherwise call `run_probe`.
2. **Get the findings.** Wait for the run to finish, then call `get_probe`. Page through with `nextOffset` until you have all of them.
3. **Triage before editing.** Group the findings:
   - property findings (text, font, colour, size): fix these first
   - spacing and layout-shift findings: leave them until after the rerun, since many disappear once the property fixes land
   - unverified findings: report them, and don't change code for them unless the user asks
4. **Fix property findings in the source.** Locate each element as the server instructions describe, change the `cssProperty` from `pageValue` to `designValue`, and prefer the project's existing design tokens over raw values when a token matches.
5. **Rerun.** Call `rerun_probe` on the same probe. For a local page, pass the current tunnel URL.
6. **Repeat** steps 2 to 5 until only findings you've reported as intentional or out of scope are left. Stop after three rounds without progress and report what remains. Findings the user calls intentional can be set aside with the `uiprobe:triage-findings` skill.

## Reporting

End with a short summary for the user:

- what you changed, with each finding's `findingUrl`
- what's left, and why you didn't change it
- the probe's viewer URL

Don't change spacing to match the design when it conflicts with the project's layout system (a grid, a spacing scale). Report the conflict instead.
