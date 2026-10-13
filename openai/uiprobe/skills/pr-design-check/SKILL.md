---
name: pr-design-check
description: Check a pull request's pages against Figma with Uiprobe and write a note for the pull request. Use before opening a pull request or asking for review, or when the user asks to check UI changes against the design before pushing. Reports where each changed page stands; it doesn't change code. Needs a code repository, for example in Codex or Claude Code.
---

# Check a pull request's pages against Figma

The Uiprobe MCP server's instructions explain each tool, how to poll a run and how to make a local page reachable. Follow them. This skill decides which pages to probe for a change and writes the result up for the reviewer.

It changes no code and dismisses nothing. To fix findings, use the `uiprobe:match-design` skill.

## Steps

1. **Find the changed pages.** Read the branch's diff and work out which pages it affects from the routes, templates and components it touches. List them for the user and ask whether to add or drop any.
2. **Pair each page with its Figma frame.** Call `list_probes` to find an existing probe for each page: match the page's route with the path of each probe's `webpageUrl`, using `nameFilter` only to narrow the list. An existing probe already links the page to its frame, its `figmaUrl`. Show the user the matches and let them correct them. For a page without a probe, ask the user for the Figma frame URL. If the user has no frame for a page, leave it out and say so in the note.
3. **Get a reachable URL.** Use the branch's preview deployment when there is one. Otherwise make the local page reachable as the server instructions describe.
4. **Say what the check will cost.** Tell the user how many runs it uses, one per page, and wait for a go.
5. **Run the probes.** Call `rerun_probe` for pages with a probe, passing the current URL, so their history is kept. Call `run_probe` for the rest. Wait for each run as the server instructions describe. If a run can't start, for example because the allowance is used up, leave that page out and say why in the note.
6. **Read the findings.** Call `get_probe` for each page and page through with `nextOffset`. The result follows the probe's finding filters and leaves out dismissed findings, the same as the web app.
7. **Write the note** in the format below. Offer to add it to the pull request description, and add it only after the user agrees. Otherwise give it to the user to paste.

## Format

```markdown
## Design check (Uiprobe)

### <page name>
Figma: <frame URL> · Page: <page URL> · Probe: <viewer URL>

Came through as designed: <one line>

Still differs:
- <what differs>: design <designValue>, page <pageValue> ([finding](<findingUrl>))

Couldn't be mapped yet: <unverified findings, one line>
```

The Figma link is the probe's `figmaUrl`, and the page link is the URL you just probed. Group findings of the same kind on one line. Leave out sections that are empty. When a page has nothing that still differs, say so in one line.

## Don't

- Fix findings or edit code. Point to the `uiprobe:match-design` skill instead.
- Start runs before the user agrees to the number of runs.
- Edit the pull request without asking.
- Judge whether the pull request is ready to merge. The note says where the pages stand; the reviewer decides.
- Compare the number of findings with an earlier run.
