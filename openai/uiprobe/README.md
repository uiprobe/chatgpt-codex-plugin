# Uiprobe plugin for ChatGPT and Codex

[Uiprobe](https://www.uiprobe.io) automatically detects spacing, typography, color, and layout differences between your Figma design and live website. This plugin connects ChatGPT and Codex to Uiprobe, so they can compare a page with its design and, in Codex, fix the differences in your code.

## Install in Codex

Add this repository as a plugin marketplace:

```
codex plugin marketplace add uiprobe/chatgpt-codex-plugin
```

Then install Uiprobe from the plugin directory and sign in with Figma when asked.

## What it adds

The plugin adds the Uiprobe MCP server at `https://app.uiprobe.io/mcp` and four skills. With the MCP server, you can:

- **Run a probe** from a Figma frame URL and a webpage URL, and get every difference in text, fonts, colors, sizes, spacing and layout.
- **Rerun a probe** after changing the page to see what's left.
- **List your probes** and read their findings.
- **Look at a finding** as crops of the design and the page.
- **Dismiss findings** you say are intentional, after asking you, so nobody on the probe sees them again, and restore them whenever you want.

The skills turn those tools into workflows. Ask for one in your own words, or call it by name:

- **`uiprobe:design-qa-summary`**: summarise a probe's findings into something you can paste for a designer, developer or PM, with images of a few of them. It only reads, and never starts a run.
- **`uiprobe:triage-findings`**: go through a probe's findings, decide what to fix and what is intentional, and dismiss the intentional ones after you confirm.
- **`uiprobe:match-design`** (Codex): make a page match its Figma design. Codex probes the page, fixes the findings in your code, and reruns the probe until the page matches.
- **`uiprobe:pr-design-check`** (Codex): before a pull request, probe the pages your branch changed against their Figma frames, after you agree to the number of runs, and get a note for the pull request description. It doesn't change code.

The plugin contains no code that runs on your machine.

## What it sends

- **To Uiprobe:** the Figma frame URL and webpage URL of each probe you run, and the ids of probes and findings you ask about. Uiprobe reads the Figma frame through Figma's API with your Figma sign-in, and captures the webpage from its own servers, so the page must be publicly reachable. For a page on localhost, Codex can open a temporary tunnel to it.
- **Sign-in:** you sign in with Figma through Uiprobe's sign-in page. ChatGPT or Codex keeps the resulting token.

Uiprobe's [privacy policy](https://www.uiprobe.io/legal/privacy) and [terms](https://www.uiprobe.io/legal/terms) apply.

## Updates

Run `codex plugin marketplace upgrade uiprobe` to get the latest version.

## Support

This repository is a published copy of the plugin and doesn't accept issues or pull requests. For help, go to https://www.uiprobe.io/help/support.

## License

MIT, see [LICENSE](https://github.com/uiprobe/chatgpt-codex-plugin/blob/main/LICENSE).
