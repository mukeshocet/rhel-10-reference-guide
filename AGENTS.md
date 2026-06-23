# AGENTS.md

## Cursor Cloud specific instructions

This repository is a **static Markdown content repo** (the "RHEL 10 Reference Guide").
It contains only Markdown files (`README.md`, `index.md`, `ex200-practice-guide*.md`).
There is **no application code, no package manager file, no automated tests, no lint
config, and no build step**. "Development" here means authoring and previewing the
Markdown so it renders cleanly on GitHub / blogs / Medium.

### Previewing the content (the closest thing to "running the app")

The dev preview tool is [`grip`](https://github.com/joeyespo/grip), a GitHub-style
Markdown renderer. It is installed into the user site (`~/.local/bin`) by the startup
update script. To preview a file:

```bash
~/.local/bin/grip ex200-practice-guide.md 0.0.0.0:6419
# then open http://localhost:6419/
```

Non-obvious caveats:
- `~/.local/bin` is **not on PATH** by default in this environment — invoke grip with
  its full path (`~/.local/bin/grip`) or add the dir to PATH.
- `grip` renders by calling **GitHub's Markdown API**, so it needs outbound network
  access and is subject to unauthenticated rate limits. For offline/local-only
  rendering, fall back to the bundled pure-Python renderer:
  `~/.local/bin/markdown_py ex200-practice-guide.md > out.html`.
