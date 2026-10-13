---
name: design-qa-summary
description: Summarise how a webpage came through against its Figma design, from a Uiprobe probe. Use when the user asks what's different between a design and a page, wants a design QA summary, or wants to share a probe's findings with a designer, developer or PM. Reads existing probes only and never starts a run.
---

# Summarise a probe's findings

The Uiprobe MCP server's instructions explain each tool and how to read a finding's evidence. Follow them. This skill turns a probe's findings into a summary a person can read and share.

It only reads. Don't call `run_probe` or `rerun_probe` unless the user asks for a new run.

## Steps

1. **Find the probe.** If the user gave a probe or viewer URL, use it. Otherwise call `list_probes`, with `nameFilter` when the user named the page or design. If more than one probe fits, ask which one.
2. **Get all findings.** Call `get_probe` and page through with `nextOffset` until you have them all. If the latest run is still `PENDING` or `RUNNING`, say so and offer to wait.
3. **Sort them into sections:**
   - **Properties**, grouped by what differs: text and content, typography, colour and style, size
   - **Spacing**, including layout shifts
   - **Unverified**, kept apart from the rest
4. **Pick a few to show.** Choose up to five findings that are easy to see on the page: grouped findings that cover many elements, and large differences in size, colour or typography. Call `get_finding_images` for them.
5. **Write the summary** in the format below.

## Format

- One line on top: the probe's title (from `list_probes`) and the viewer URL.
- One short paragraph on what came through as designed.
- Per section, one bullet per finding or group: what differs, the design value and the page value, and the `findingUrl`. Put a group on one line and say how many elements it covers.
- The images from step 4, each under its bullet.
- Unverified findings last, as elements Uiprobe couldn't map yet. Don't present them as differences.
- If `get_probe` reports findings hidden by the probe's filters or dismissed, say how many and offer to include them.

Keep it short enough to paste into a chat. Offer a longer version if the user wants every finding.

## Don't

- Rank findings by importance or severity. Uiprobe reports differences; the user decides what matters.
- Explain why a difference exists. You can't see the code or the design process behind it.
- Compare the number of findings with an earlier run.
