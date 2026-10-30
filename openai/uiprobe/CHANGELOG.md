# Changelog

## 0.1.0

### Features

- Four skills: `uiprobe:design-qa-summary` summarises a probe's findings to share and `uiprobe:triage-findings` goes through findings with you and dismisses the intentional ones, in ChatGPT and Codex. In Codex, `uiprobe:match-design` brings a page in line with its Figma design and `uiprobe:pr-design-check` checks a pull request's pages against Figma and writes a note for the pull request.

### Submissions

- 2026-10-14: submitted to OpenAI's plugin directory.
- 2026-10-28: rejected: "One or more of your tool's annotations do not appear to match the tool's behavior." The server now marks `dismiss_findings` as destructive.
